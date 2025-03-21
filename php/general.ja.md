# PHP コーディング規約

## 基本規格

### PSR 規格
- **PSR-12**（拡張コーディングスタイル） - これはPSR-2を置き換えるもので、主要なスタイルガイドとして採用すべきです
  - [PSR-12](https://www.php-fig.org/psr/psr-12/)
  - PSR-2が作成された後に導入された新しいPHP機能に対応した更新が含まれています
- **PSR-1**（基本コーディング規格） - 基礎的なコーディング規格
  - [PSR-1](https://www.php-fig.org/psr/psr-1/)
- **PSR-4**（オートローディング規格） - クラスのオートローディング規約
  - [PSR-4](https://www.php-fig.org/psr/psr-4/)

### クライアント固有の規格
- クライアントが提供するコーディング規約がある場合はそれに適応する
- ただし、PSR-12のような一般的に受け入れられているコーディング規約に合わせることは開発チームとして有益です
- クライアントの規格がPSR規格と大きく異なる場合は、クライアントとの協議が必要です

## 追加の規約

### 命名規則
- **クラス**: パスカルケース（例: `UserAuthentication`）
- **メソッド/関数**: キャメルケース（例: `getUserById()`）
- **変数**: キャメルケース（例: `$userEmail`）
- **定数**: アッパースネークケース（例: `MAX_LOGIN_ATTEMPTS`）
- **プライベート/プロテクテッドプロパティ**: アンダースコアを接頭辞として検討（例: `$_internalState`）
- **名前空間**: パスカルケース（例: `App\Services\Authentication`）
- **ファイル名**: 大文字小文字を含めてクラス名と一致させる（例: `UserAuthentication.php`）

### コード構造
- 1ファイルにつき1クラス
- 関連するクラスを名前空間でグループ化する
- 名前空間の階層を反映した論理的なディレクトリ構造を整理する
- メソッドは小さく、単一の責任に焦点を当てる
- ネストのレベルを制限する（最大3〜4レベル）
- 「マジックナンバー」を避け、代わりに名前付き定数を使用する
- 可読性と一貫性、パフォーマンス対策のために、`array()`よりも短い配列構文`[]`を使用する
  ```php
  // 推奨
  $items = ['apple', 'banana', 'orange'];
  
  // 避ける
  $items = array('apple', 'banana', 'orange');
  ```

### ドキュメンテーション
- クラス、メソッド、プロパティにはPHPDocコメントを使用する
- docblockに型ヒントを含め、可能な場合はPHP 7.4+の型付きプロパティを使用する
- パラメータ、戻り値の型、例外をドキュメント化する
- 複雑なロジックには意味のあるコメントを追加する
- 著作権情報と簡単な説明を含むファイルヘッダーを含める

```php
/**
 * ユーザー認証サービス
 *
 * ユーザーのログイン、登録、セッション管理を処理します
 *
 * @package App\Services
 */
class AuthenticationService
{
    /**
     * ユーザー認証を試みる
     *
     * @param string $email ユーザーのメールアドレス
     * @param string $password ユーザーのパスワード（平文）
     * @return User|null 認証されたユーザー、または認証に失敗した場合はnull
     * @throws AuthenticationException 認証サービスが利用できない場合
     */
    public function authenticate(string $email, string $password): ?User
    {
        // 実装
    }
}
```

### エラー処理
- 戻り値コードではなく例外を使用してエラーを処理する
- 異なるエラータイプに対してカスタム例外クラスを作成する
- 例外を適切にログに記録する
- `@`演算子でエラーを抑制することを避ける

### セキュリティプラクティス
- 常にユーザー入力を検証およびサニタイズする
- データベースクエリにはプリペアドステートメントを使用する
- 適切なパスワードハッシュを実装する（`password_hash()`と`password_verify()`を使用）
- OWASPセキュリティガイドラインに従う
- エラーメッセージで機密情報を公開しない

### パフォーマンスの考慮事項
- 可能な限り厳密な比較演算子（`===`、`!==`）を使用する
- ループや大きなデータセットでのメモリ使用に注意する
- 高コストな操作にはキャッシュを検討する
- タスクに適したデータ構造を使用する

## 強制ツール

### 静的解析
- **PHP_CodeSniffer**: PSR-12とカスタムルールを強制するため
  - プロジェクトに`phpcs.xml`設定ファイルを含める
- **PHPStan**: 静的コード解析のため
  - レベル5から始めて徐々に上げていく
- **Psalm**: PHPStanの代替として、いくつかの異なる機能を持つ

### CI統合
- CI/CDパイプラインの一部としてコーディング規格チェックを実行する
- 規格を満たさないビルドを失敗させる
- ローカルでの強制のためにpre-commitフックの使用を検討する

### IDE設定
- IDE（PhpStorm、VS Code）がコーディング規格違反を強調表示するように設定する
- プロジェクトリポジトリでIDE設定ファイルを共有する

## バージョン固有の考慮事項
- PHP 7.4+の場合: 型付きプロパティを使用する
- PHP 8.0+の場合: コンストラクタプロパティプロモーション、名前付き引数、属性を使用する
- PHP 8.1+の場合: 列挙型、読み取り専用プロパティ、ファーストクラスのcallable構文を使用する

## フレームワーク固有のガイドライン
- Laravelを使用する場合、PSRと異なる部分ではLaravelの規約に従う
- Symfonyを使用する場合、PSRと異なる部分ではSymfonyの規約に従う
- 一般的なルールに対するフレームワーク固有の例外をドキュメント化する

### WordPress の考慮事項
- WordPressプロジェクトで作業する場合は、[WordPress PHP コーディング規約](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/)に従う
- 関数やフックにはWordPressの命名規則（スネークケース）を使用する
- 競合を避けるため、関数、クラス、グローバル変数にはプロジェクト固有の接頭辞を付ける
- 直接SQLクエリではなく、WordPressの組み込み関数を使用してデータベース操作を行う
- WordPressのセキュリティベストプラクティスに従う：
  - データの検証、サニタイズ、エスケープを行う（`sanitize_text_field()`、`esc_html()`、`esc_url()`などの関数を使用）
  - フォーム送信にはnoncesを使用する
  - ユーザー権限にはケイパビリティチェックを使用する
- WordPressの関数を使用して、スクリプトとスタイルを適切にエンキューする
- コアファイルを変更する代わりに、WordPressのフック（アクションとフィルター）を使用する

## コードレビュープロセス
- コードレビューチェックリストの一部としてコーディング規格の遵守を含める
- 自動化できるものは自動化し、手動レビューはロジックと設計に集中する
- コーディング規格に関するフィードバックに一貫したアプローチを維持する