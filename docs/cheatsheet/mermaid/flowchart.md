# Flowchart
Frowchartでの図式表現についてまとめる

## ノード(node)
```mermaid
flowchart TD
A[①あいうえお]
B(②あいうえお)
C([③あいうえお])
D((④あいうえお))
N(((⑤あいうえお)))
E{⑥あいうえお}
K{{⑦あいうえお}}
```
```mermaid
flowchart TD
L[/⑧あいうえお\]
M[\⑨あいうえお/] 
F[/⑩あいうえお/]
G[\⑪あいうえお\]
H[[⑫あいうえお]]
If>⑬あいうえお]
Jd[(⑭あいうえお)]
```
| 番号 | 記法 | 形 | 用途 | 備考 |
|----|--------|--------|--------|-----------|
| 1  | [] | 四角 | 処理 | |
| 2  | () | 丸 | 開始/終了 | これ全然丸じゃないよねｗ |
| 3  | ([]) | 角丸 | | スタジアムノードっていうらしい |
| 4  | (()) | 二重〇 | 強調 ||
| 5  | {} | ひし形 | 条件 | |
| 6  | [//] | 平行四辺形 | 入出力 | |
| 7  | [\　\\] | | | |
| ８  | [[]] | サブルーチン | サブルーチン | |
| ９  | f>] | 非対称型 | | 左側や両方はできない | |
| 10 | d[()] | DB | DB |

## リンク

```mermaid
flowchart LR
A-->|-->|B<-->|<-->|F
A---|---|C--o|--o|G
A==>|==>|D--x|--x|H
A-.->|-.->|E-.-|-.-|I
z--テキスト-->x[--テキスト-->]
```

## 向き

> TD　上→下
> 
> BT　下→上
> 
> LR　左→右
> 
> RL　右→左

## ノードにIDをつける

```mermaid
flowchart  TD
aaaaaa[ID:aaaaaa]
これもOK[ID:これもOK]

    aaaaaa-->これもOK
```

>**以下のように好きなIDを振ることができる**
>>
>>aaaaaa[ID:aaaaaa]
>>
>>これもOK[ID:これもOK]
>>
>>aaaaaa-->これもOK

## サブグラフ

```mermaid
flowchart TD
A---C
B---C

subgraph サブグラフ
D---E
F---E
end

F---A

B & D---H/
```

## ディレクション

```mermaid
    flowchart LR 
    A[あいうえお]
    E[なにぬねの]
subgraph sub
    direction TB
    B[かきくけこ]
    C[さしすせそ]
    D[たちつてと]
end

```


## チャッピーに問題を出してもらった

```mermaid
flowchart LR
    
subgraph Frontend
    A[Browser]
    B[React]
end

subgraph Backend
    C[Controller]
    D[Service]
    E[Mapper]
end

    F[(MySQL)]

A-->B-->C-->D-->E
E-->F
```
```mermaid
    flowchart TD
        subgraph AWS
            subgraph EC2
                N[NginX]-->S[SpringBoot]
                end
        subgraph RDS
            M[(MySQL)]
        end

        end

S-->M
```

```mermaid
    flowchart LR

    subgraph Client
        direction LR
        u[User]--> b[Browser]
    end
    
    subgraph Server
        direction TB
        
        subgraph App
            direction TB
            c[Controller]-->s[Service]
        end
        
        subgraph db 
            direction TB
           s-->My[(MySQL)]
        end
    end
    
    b-->c
    
```





## 参考
>[【まとめ】mermaid図式表現 ＞フローチャート編｜ふじけん先生｜note](https://note.com/rapid_phlox5979/m/m732d1880da7b)


## 更新履歴
2026-2-20　新規作成