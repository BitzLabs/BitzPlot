# BitzPlot

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzPlotは、BitzLabsエコシステム内で宣言的に統計チャートを定義し、描画するための専門ライブラリです。Vega-Liteの思想に強くインスパイアされています。

## 主な機能

-   **`PlotAST`の定義**: データ、マーク(Mark)、エンコーディング(Encoding)という概念に基づいた、宣言的なチャート定義のためのAST。
-   **2つの入力ルート**:
    1.  **パーサー**: YAMLやJSON風の簡易言語を解釈し、`PlotAST`を生成します。
    2.  **専用API (Fluent API)**: C#やTypeScriptのコードから、データフレームやオブジェクトのリストを元に`PlotAST`を動的に構築できます。
-   **レイアウトエンジン**: `PlotAST`を解釈し、軸の計算、スケールの適用、データポイントのマッピングを行い、最終的にチャートを構成する描画プリミティブの集合である**`LayoutAST`**を生成します。

## ✅ 初期開発ToDoリスト

1.  **`PlotAST`の型定義**:
    *   `IPlotNode`インターフェース(`IAstNode`継承)を定義。
    *   `PlotNode`, `LayerNode`, `MarkNode`, `EncodingNode`など、主要なASTノードクラスの「殻」を作成。
2.  **専用API (ビルダー) の設計**:
    *   `PlotBuilder`クラスを作成。`.WithMark(MarkType.Bar)`, `.WithEncoding(...)`のようなメソッドチェーン(Fluent API)で`PlotNode`を構築できるインターフェースを設計。
3.  **パーサーの骨格**:
    *   `PlotParser`クラスを作成。
    *   `Parse(string yamlOrJson)`メソッドを実装。YAML/JSONライブラリを使い、最もシンプルな棒グラフの定義をパースして`PlotNode`にマッピングするロジックを実装。
4.  **レイアウトエンジンのインターフェース**:
    *   `PlotLayoutEngine`クラスを作成し、`Render(IPlotNode node)`メソッドを定義。
    *   初期実装として、単純な棒グラフの`PlotAST`を受け取り、軸と矩形を描画するための`LayoutAST`（`LineNode`と`RectNode`の集まり）を生成するロジックの骨格を実装。

## このライブラリの位置づけ

このライブラリは、主に`BitzDoc`から利用され、Markdown文書内に棒グラフ、折れ線グラフ、散布図などを埋め込む機能を提供します。また、データ分析結果を可視化するためのSDKとしても活用できます。

このライブラリの最終的な出力は`LayoutAST`です。これをSVGなどの具体的な画像形式に変換する処理は、`BitzRenderers`に委譲されます。

### 利用シナリオ

-   **DSL経由**: ドキュメント内に静的なマーケティングレポート用チャートを埋め込む。
-   **API経由**: データベースから取得した最新の販売データをリアルタイムにグラフ化するダッシュボードのバックエンドを構築する。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`
-   `BitzLayout` (出力型として)
