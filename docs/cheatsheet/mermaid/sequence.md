# Sequence
SequenceDiagramでの図式表現についてまとめる

複合フラグメントはＵＭＬに準拠した様々な記法があるが、ここではＵＭＬの学習よりもMermaidの学習にウェイトがあるため主要なものだけにする。
機会があればＵＭＬについて学習したいので別issueとする

## 基本
```mermaid
sequenceDiagram
  Alice->>+Bob: メッセージ
  note over Alice:ライフライン
  note right of Bob:←実行バー(Activation/Execution specification)
  Bob-->>-Alice: メッセージ(戻り)
```

## participant / actor

```mermaid
    sequenceDiagram
        actor 俺
        participant  ネコ
        participant ネコ②
        
        俺->>ネコ:きみかわいいね
        ネコ->>ネコ②:ムシしようぜ
        ネコ②-->>俺:どっかいけよ
        ネコ-->>俺:るせぇどっかいけ
```
>sequenceDiagram
> 
>actor 俺
> 
>participant  ネコ
> 
>participant ネコ② 
>
>俺->>ネコ:きみかわいいね
> 
>ネコ->>ネコ②:ムシしようぜ
> 
>ネコ②-->>俺:どっかいけよ
> 
>ネコ-->>俺:るせぇどっかいけ

## self call

```mermaid
sequenceDiagram
  participant Service
  Service->>Service: validate()
  俺->>俺:自問自答
```
>sequenceDiagram 
> 
>participant Service
> 
>Service->>Service: validate()
> 
>俺->>俺:自問自答

## メッセージの種類

```mermaid
sequenceDiagram
    participant A
    participant B

    A->>B: A->>B
    A-->>B: A-->>B
    A-xB: A-xB
    A-)B: A-)B
```

## 注釈

```mermaid
sequenceDiagram
    participant A
    participant B
    participant C

    Note over B:note over B
    A->>B:こうやって
    Note left of A:Note left of A
    note right of C:note right of C
    B->>C:こう
    note over B,C:note over B,C
    note over A,C:note over A,C
    C-->>A:戻り
    
```

## 複合フラグメント(Combined Fragment)

| 記法   | 意味          | 用途              |
|------|-------------|-----------------|
| alt  | Alternative | 分岐　認証・バリデーションなど |
| opt  | Optional    | 任意処理　キャッシュなど    |
| loop | Loop        | リトライ・ページング      |
| par  |Parallel| 並列処理　通知・ログなど    |


### alt / else

```mermaid
sequenceDiagram
    
    actor 俺
    participant ネコ
    
    俺->>ネコ:うちにこない？
    alt 成功 
        ネコ-->>俺:お世話になりますにゃ
    else 失敗
        ネコ-->>俺:お断りしますにゃ
    end
    
    

```

### opt

```mermaid
sequenceDiagram
  participant Browser
  participant API
  participant DB

    Browser-->>API:リクエスト送信
  opt キャッシュなしの場合
    API->>+DB: SELECT ...
    DB-->>-API: rows
  end
```
### loop    

```mermaid
sequenceDiagram
  participant Service
  participant ExternalAPI

  loop 3回まで再試行
    Service->>ExternalAPI: request
    alt 成功
      ExternalAPI-->>Service: 200 OK
    else 失敗
      ExternalAPI-->>Service: error
    end
  end
```
### par

```mermaid
sequenceDiagram
  participant API
  participant Mail
  participant Log

  par メール送信
    API->>Mail: send()
  and ログ記録
    API->>Log: write()
  end

  API-->>Client: 201 Created
```

## destroy


```mermaid
sequenceDiagram
  participant A
  participant B

  A->>B: do something
  destroy B
  
  B-->>A:aaa
  
```

## チャッピーからの問題

```mermaid
sequenceDiagram
    actor User
    participant LoginController
    participant AuthService
    participant UserRepository
    
    User->>+LoginController:ログイン
    LoginController->>+AuthService:認証依頼
    AuthService->>+UserRepository:ユーザー検索
    UserRepository-->>-AuthService:UserEntity or null
    
    alt ユーザーあり&PW一致
        AuthService-->>LoginController:ログイン成功
    else ユーザーなし
        AuthService-->>LoginController:エラーレスポンス
    else PW不一致
        AuthService-->>LoginController:エラーレスポンス
    end

    LoginController-->>-User:結果を返す
```

```mermaid
sequenceDiagram
    actor User
    participant LoginController
    participant AuthService
    participant LoginAttemptService
    participant AccountLockService
    participant UserRepository
    

    loop ３回まで
        User->>+LoginController:ログイン
        LoginController->>+AuthService:認証依頼
        AuthService->>+LoginAttemptService:認証依頼
        LoginAttemptService->>+UserRepository:ユーザー検索
        UserRepository-->>-LoginAttemptService:UserEntity or null
        alt 認証成功 
            LoginAttemptService-->>AuthService:OK
            AuthService-->>LoginController:OK
            LoginController-->>User:ログイン成功画面
            else 認証失敗
            LoginAttemptService->>LoginAttemptService:失敗回数+1
            alt <=3 
                LoginAttemptService-->>AuthService:再入力
                AuthService-->>LoginController:再入力
                LoginController-->>User:再入力
                else >=3
                LoginAttemptService->>+AccountLockService:Lock
                AccountLockService-->>-LoginAttemptService:Locked
                LoginAttemptService-->>-AuthService:Locked
                AuthService-->>-LoginController:Locked
                LoginController-->>-User:401
            end
        end
        
    end
```

```mermaid
sequenceDiagram
    actor User
    participant LoginController
    participant AuthService
    participant  AuditLogService
    participant  SessionService
    participant  UserRepository
    
    User->>+LoginController:ログイン
    LoginController->>+AuthService:認証依頼
    AuthService->>+UserRepository:ユーザ検索
    UserRepository-->>-AuthService:UserEntity　or null
    alt 認証成功

        AuthService->>+SessionService:Sessionの作成
        SessionService-->>-AuthService:Session
        AuthService-->>LoginController:Session  
        LoginController-->>User:画面
        par 平行処理
        note over AuditLogService,SessionService:以降は(副作用非同期/レスポンス後)
        AuthService-)AuditLogService:ログの記録
        and
        AuthService-)UserRepository:ログイン日時の更新
        end
    else 認証失敗
        AuthService-->>-LoginController:401
        LoginController-->>-User:401
    end

```

```mermaid
sequenceDiagram
    actor User
    participant LoginController
    participant AuthService
    participant LoginAttemptService
    participant AccountLockService
    participant UserRepository
    participant SessionService
    participant AuditService
    participant InternalLogger
    
    loop ３回まで 
        User->>+LoginController:ログイン
        LoginController->>+AuthService:認証依頼
        AuthService->>+LoginAttemptService:認証依頼
        LoginAttemptService->>+UserRepository:ユーザ検索
        UserRepository-->>-LoginAttemptService:UserEntity or null
        LoginAttemptService->>LoginAttemptService:認証判定

        
        alt 認証成功
            LoginAttemptService-->>AuthService:OK
            AuthService->>+SessionService:Session作成
            SessionService-->>-AuthService:Session
            AuthService-->>LoginController:Session
            LoginController-->>User:画面
            par 平行処理
                AuthService->>AuditService:監査ログ
                AuthService->>UserRepository:ログイン日時更新
                
            end
            opt parでの処理が失敗した場合
                AuthService->>InternalLogger:監査ログ失敗
                AuthService->>InternalLogger:最終ログイン更新失敗
            end
        else 認証失敗
            LoginAttemptService->>LoginAttemptService:failCount++
            alt failcount < 3
                LoginAttemptService-->>AuthService:401
                AuthService-->>LoginController:401
                LoginController-->>User:401
            else failcount >= 3
                LoginAttemptService->>+AccountLockService:Lock
                AccountLockService-->>LoginAttemptService:423 Locked
                LoginAttemptService-->>-AuthService:423 Locked
                AuthService-->>-LoginController:423 Locked
                LoginController-->>-User:423 Locked
                opt 処理失敗時
                    AccountLockService->>-InternalLogger:ログの作成
                end
            end
        end        
    end
    
```








## 参考
>[【SequenceDiagrams】シーケンス図の書き方 #uml - Qiita](https://qiita.com/dodomasaki/items/31e2210c07cb58471ac4)


## 更新履歴
2026-2-20:初回作成

2026-2-25:## チャッピーからの問題　を記載