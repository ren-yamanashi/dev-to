# Zenn から dev.to への記事翻訳ルール

## 概要

このプロジェクトは、日本語の [Zenn](https://zenn.dev/) 記事を英語の [dev.to](https://dev.to/) 記事に翻訳・変換するためのものである

## 翻訳ルール

### 1. 基本的な翻訳方針

- 技術用語は英語のまま維持する (e.g. AWS CDK, TypeScript, API)
- 日本語特有の表現は、英語圏の読者に理解しやすい表現に変換する
- コードブロックやコマンドはそのまま維持する
- 記事のトーンは professional かつ friendly に保つ

### 2. Markdown 記法の変換

#### 2.1 メッセージ記法の変換

Zenn の特殊記法を dev.to で利用可能な形式に変換する

**Zenn のメッセージ記法**

```markdown
:::message
メッセージ内容
:::

:::message alert
警告メッセージ
:::
```

**dev.to への変換**

```markdown
> ℹ️ **Note:** Message content

> ⚠️ **Warning:** Alert message
```

<br />

#### 2.2 アコーディオン (詳細) 記法の変換

**Zenn のアコーディオン**

```markdown
:::details タイトル
詳細内容
:::
```

**dev.to への変換**

```markdown
<details>
<summary>Title</summary>

Detail content

</details>
```

<br />

#### 2.3 リンクカード記法の変換

**Zenn のリンクカード**

```markdown
@[card](URL)
```

**dev.to への変換**

```markdown
{% embed URL %}
```

<br />

#### 2.4 Twitter/X 埋め込みの変換

**Zenn**

```markdown
@[tweet](ツイートURL)
```

**dev.to への変換**

```markdown
{% twitter ツイートURL %}
```

<br />

#### 2.5 YouTube 埋め込みの変換

**Zenn**

```markdown
@[youtube](動画ID)
```

**dev.to への変換**

```markdown
{% youtube 動画ID %}
```

<br />

#### 2.6 数式記法の変換

**Zenn のインライン数式**

```markdown
$a = b + c$
```

**dev.to への変換（インライン）**

```markdown
$a = b + c$
```

**Zenn のブロック数式**

```markdown
$$
a = b + c
$$
```

**dev.to への変換（ブロック）**

```markdown
$$
a = b + c
$$
```

<br />

### 3. フロントマターの変換

**Zenn のフロントマター例**

```yaml
---
title: "記事タイトル"
emoji: "🚀"
type: "tech"
topics: ["react", "typescript"]
published: true
---
```

**dev.to のフロントマター例:**

```yaml
---
title: Article Title
published: true
tags: react, typescript, javascript, webdev
cover_image: https://example.com/cover.jpg
description: Brief description of the article
series: Series Name (optional)
canonical_url: https://zenn.dev/original-url
---
```

### 4. 翻訳時の注意事項

#### 4.1 タイトルとメタデータ

- topics は tags に変換（最大 4 つまで）
- canonical_url に元の Zenn 記事の URL を設定
- description を追加（記事の概要を英語で記載）

#### 4.2 画像の処理

- Zenn にアップロードされた画像は URL をそのまま使用
- alt テキストは必ず英語に翻訳

#### 4.3 コードブロック

- コードのコメントは英語に翻訳
- ファイル名の記法はそのまま維持

#### 4.4. 第一人称の統一

- 日本語記事で「私」や「僕」が使われている場合、英語では "I" を使用
- 一人称が省略されている場合、基本的には `We` ではなく `I` を使用
  - ただし、チームや組織を示している場合は `We` を使用
  - どの一人称を利用すれば良いか不明な場合は、予測せず、常にユーザーに確認すること

### 5. SEO とアクセシビリティ

- 見出し（h1, h2, h3）の階層構造を適切に保つ
- 画像には必ず alt テキストを設定
- リンクテキストは説明的にする（"click here" は避ける）

### 6. dev.to 特有の機能の活用

- シリーズ記事の場合は series フィールドを設定
- 関連記事へのリンクは dev.to の記法を使用
- Liquid タグを活用してインタラクティブな要素を追加可能
