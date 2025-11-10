# Zenn 記事を dev.to へ翻訳

GitHub 上の Zenn 記事を取得し、英語に翻訳して dev.to 形式に変換してください。

## 入力

GitHub URL: $ARGUMENTS

## 実行手順

1. 記事の取得
   - 提供された URL を raw URL に変換することで、Markdown コンテンツを取得
     - 提供された URL の形式: `https://github.com/ren-yamanashi/zenn/blob/main/articles/[記事ID].md`
     - raw URL の形式: `https://raw.githubusercontent.com/ren-yamanashi/zenn/main/articles/[記事ID].md`
       - `curl -L ${raw_url}` を使用して記事を取得

2. 翻訳
   - @CLAUDE.md に記載された翻訳ルールに従って日本語から英語への翻訳を行う

3. ファイル作成
   - 変換後の記事を `articles/` ディレクトリに保存
   - ファイル名: `devto-[元の記事ID].md`
