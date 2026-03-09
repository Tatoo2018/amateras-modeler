Amateras Modeler (Fork for CI/CD)

本リポジトリは、takezoe/amateras-modeler をベースに、GitHub Actions による更新サイトの自動ビルドおよびデプロイを確認・実施するための環境です。

🚀 更新サイト (Update Site)

以下のURLをEclipseの「Install New Software」に登録することで、最新のビルドをインストール可能です。

https://tatoo2018.github.io/amateras-modeler/updatesite/master/

🛠 実施した作業内容

最新のビルド環境（JDK 17/21等）でのビルドおよび自動デプロイを実現するため、以下の変更を行いました。

1. ビルド環境のアップグレード

オリジナルの環境設定（JavaSE-1.6）を、現代的な開発環境での動作を考慮し Java 1.8 へ引き上げました。

2. Maven (Tycho) プロジェクトへの変換

各EclipseプロジェクトをMavenプロジェクト形式へ変換。

親プロジェクトおよび各サブモジュールの pom.xml を設定。

eclipse-repository パッケージングを用いた自動パッケージングの構成。

3. コンパイルエラーの修正

Java 1.8以降での厳格な型チェックおよびビルド環境の差異に対応するため、以下のGEF関連クラスのソースコードを修正しました。

修正対象ファイル:

net.java.amateras.db/src/net/java/amateras/db/visual/action/CopyAction.java

net.java.amateras.db/src/net/java/amateras/db/visual/action/Logical2PhysicalAction.java

net.java.amateras.db/src/net/java/amateras/db/visual/action/LowercaseAction.java

net.java.amateras.db/src/net/java/amateras/db/visual/action/Physical2LogicalAction.java

net.java.amateras.db/src/net/java/amateras/db/visual/action/UppercaseAction.java

修正内容:
GEF APIの戻り値の型変更（あるいは未定義ジェネリクス）に伴う代入エラーを回避。

// Before
List<EditPart> selection = getSelectedObjects();

// After
List<Object> selection = getSelectedObjects();


4. インストール構成の改善

更新サイトからのインストール時に、Eclipse上でプラグインを正しく認識・整理させるため、category.xml を新規追加しました。

カテゴリー名 「TOOL」 を定義し、ユーザーがフィーチャーを見つけやすいように改善しました。

5. 自動デプロイの設定

GitHub Actionsにより、master（または main）ブランチへのプッシュ時に自動的にビルドが走り、成果物が gh-pages ブランチの所定のパスへデプロイされる仕組みを構築しました。

📦 開発者向け情報

本リポジトリをビルドするには、以下のコマンドを実行してください。

mvn clean verify


ビルド完了後、net.java.amateras.updatesite/target/repository 内にP2リポジトリ（更新サイト）が生成されます。

ライセンス

Eclipse Public License 1.0 に準拠します。
