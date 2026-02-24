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






## 参考
>[【SequenceDiagrams】シーケンス図の書き方 #uml - Qiita](https://qiita.com/dodomasaki/items/31e2210c07cb58471ac4)