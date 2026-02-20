<h1> 📖 dolshin-design-system</h1>

<div align="center">

![Base UI](https://img.shields.io/badge/Base%20UI-1.1.0-6366F1?style=flat-square)
![Vanilla Extract](https://img.shields.io/badge/Vanilla%20Extract-1.17.5-F786AD?style=flat-square&logo=vanillaextract)
![Style Dictionary](https://img.shields.io/badge/Style%20Dictionary-5.1.1-0EA5E9?style=flat-square)
![Figma](https://img.shields.io/badge/Figma-Design-F24E1E?style=flat-square&logo=figma)

<br/>

**Gatsby から Next.js への移行を契機に構築した個人開発用デザインシステム**
<br/>

**現在は、APIが安定していないため、ソースコードは別リポジトリで管理しています**
<br/>
<br/>

<a href="https://github.com/dolshin/gallery-aoi-next">👉ギャラリーアオイモノレポ</a>

</div>


<h2> 📋 目次 </h2>

- [✨ プロジェクト概要](#-プロジェクト概要)
- [📚 デモ](#-デモ)
- [📖 コンポーネント一覧](#-コンポーネント一覧)
- [🛠️ 技術スタック](#️-技術スタック)
- [🏗️ アーキテクチャ](#️-アーキテクチャ)
- [💡 設計上のポイント](#-設計上のポイント)
- [🔭 今後の展望](#-今後の展望)



## ✨ プロジェクト概要

Gatsby から Next.js への移行を進めていく中で、下記の課題を感じたため、このプロジェクトを作成しました。<br/>

- **スタイル定義が属人化しており、統一したスタイルを提供できていない**
- **汎用的なコンポーネントをプロジェクトごとに一から作成しないといけない**


## 📚 デモ

下記のリンクから Storybook をご覧いただけます。<br/>
UI コンポーネントの状態やバリエーションを確認できます。<br/>
👉 [Storybook デモ](https://6971793da7deabdbd1160a09-wnbvfpdomw.chromatic.com/)



## 📖 コンポーネント一覧
作成済のコンポーネント一覧は下記のとおりです。
- **Chip**
- **Typography**
- **TextField**
- **TextAreaField**
- **SelectField**
- **Button**
- **Pagination**

## 🛠️ 技術スタック
長期に渡って運用できるように、下記の技術を選定しました。
| カテゴリ | 技術 | 選定理由 |
| --- | --- | --- |
| UI ベース | Base UI |振る舞い・アクセシビリティを委譲するため |
| スタイリング | Vanilla Extract |型安全なスタイリング|
| デザイントークン | Token Studio | Figma 上でトークンを一元管理するため |
| トークンビルド | Style Dictionary | トークンの変換と整形|
| ビルド | Vite  | 高速なビルド（ESM）|
| ドキュメント | Storybook | ドキュメントとデモ|
| アイコン | Lucide / Simple Icons | オープンソースのアイコンライブラリ|


## 🏗️ アーキテクチャ


<h3> 全体像 </h3>

デザインの Source of Truth は Figma、トークン構造の Source of Truth は Theme Contract で管理しています。<br/>

```
1. Token Studio  　 トークン管理（DTCG 形式）
        ↓　
2. Style Dictionary　トークンの変換と整形
        ↓
3. Theme Contract    テーマ契約を定義
        ↓
4. UI Component      UIコンポーネント実装
        ↓
5. Form Component　  フォームコンポーネント実装
```

<h3>パッケージの依存関係</h3>

各パッケージの依存の向きを**単方向**になるように設計しています。<br/>

```
@dolshin/forms　→　@dolshin/ui →　@dolshin/theme-contract　→　@dolshin/design-token

@dolshin/ui　→　@dolshin/icons
```

<h3>パッケージ構成</h3>

各パッケージを**単一責務**となるように設計しています。<br/>

| パッケージ | 責務 | 使用ライブラリ |
| --- | --- | --- |
| @dolshin/design-token | トークン定義・ビルド | Token Studio / Style Dictionary |
| @dolshin/icons | SVG アイコン管理 | Lucide / Simple Icons |
| @dolshin/theme-contract | theme contract 定義 | Vanilla Extract |
| @dolshin/ui | UI コンポーネント | Base UI |
| @dolshin/forms | フォームコンポーネント | RHF / Conform |


## 💡 設計上のポイント

<h3>三層構造のトークン設計</h3>


| トークン種別 | 役割 | 例 |
| --- | --- | --- |
| Primitive Token | 物理的な値 | #fff , 2.5rem |
| Semantic Token | 意味を持つ抽象トークン | primary / error |
| Component Token | UI 固有のトークン | button / input |

<h3>依存方向</h3>

**コンポーネントトークンはセマンティックトークンのみを参照し、直接プリミティブトークンは参照しない。**<br/>
**これにより、デザイン変更時の影響を意味単位に閉じ込めることができます。**
```
1. Primitive Token 
        ↑　
2. Semantic Token
        ↑
3. Component Token
```

<h3>例</h3>

```ts
backgroundColor: vars.color.primary.bg
borderRadius: vars.radius.pill
```

<h3>ポイント</h3>

- **意味単位でスタイリングを切り替えられる**

- **UI 実装が具体的な値を直接知らなくて済む**


## 🔭 今後の展望

実際に個人開発のプロジェクトに組み込んでみて、下記のような課題が見つかりました。<br/>
今後はこれらの点を改善し、より使いやすいデザインシステムになるように研鑽を続けていきます。

- **セマンティックトークン・コンポーネントトークンの命名が統一されていない**
- **カラースケールやタイポグラフィースケールなどの値を、情報設計をもとに定義できていない**

