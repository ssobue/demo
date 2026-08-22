# Spring Boot Java テンプレート

[English](README.md)

このリポジトリは、Spring Boot アプリケーションのひな型です。
テンプレートからリポジトリを作成した後にビルドシステムを選べるよう、同等の Maven 設定と Gradle 設定を収録しています。

二つの設定は同じアプリケーションを定義します。
派生したプロジェクトで、両方のビルド設定を並行して保守することは想定していません。

## 必要な環境

- JDK 25
- macOS または Linux で `mvnw` や `gradlew` を実行するための POSIX 互換シェル
- Windows で `mvnw.cmd` や `gradlew.bat` を実行するための PowerShell またはコマンドプロンプト

Wrapper が必要な Maven または Gradle を取得するため、ビルドツールをシステム全体へインストールする必要はありません。

## テンプレートからプロジェクトを作成する

1. GitHub の **Use this template** からリポジトリを作成します。
2. 作成したリポジトリをクローンします。
3. サンプルの座標、パッケージ、アプリケーション名を変更します。
4. Maven または Gradle を選び、選択しなかったビルドシステムのファイルを削除します。
5. 選択したビルドシステムでビルドします。

少なくとも、次のサンプル値を変更してください。

- Java パッケージとビルド設定の `dev.sobue.demo`
- アーティファクト名、ルートプロジェクト名、`spring.application.name` の `demo`
- プロジェクトの初期バージョンが異なる場合は `0.0.1-SNAPSHOT`

## ビルドシステムを選ぶ

### Maven

次のファイルとディレクトリを残します。

- `pom.xml`
- `.mvn/`
- `mvnw`
- `mvnw.cmd`
- `.github/workflows/maven.yml`

次の Gradle 用ファイルとディレクトリを削除します。

- `build.gradle`
- `settings.gradle`
- `gradle/`
- `gradlew`
- `gradlew.bat`
- `.github/workflows/gradle.yml`

アプリケーションをビルドして実行します。

```sh
./mvnw --batch-mode verify
./mvnw spring-boot:run
```

### Gradle

次のファイルとディレクトリを残します。

- `build.gradle`
- `settings.gradle`
- `gradle/`
- `gradlew`
- `gradlew.bat`
- `.github/workflows/gradle.yml`

次の Maven 用ファイルとディレクトリを削除します。

- `pom.xml`
- `.mvn/`
- `mvnw`
- `mvnw.cmd`
- `.github/workflows/maven.yml`

アプリケーションをビルドして実行します。

```sh
./gradlew build
./gradlew bootRun
```

## ネイティブイメージ

ネイティブ実行ファイルのビルドには GraalVM が必要です。

Maven では次のコマンドを実行します。

```sh
./mvnw --batch-mode -Pnative native:compile -DskipTests
```

Gradle では次のコマンドを実行します。

```sh
./gradlew nativeCompile
```

## 継続的インテグレーション

テンプレートには、対応する Maven と Gradle のワークフローがあります。
どちらのワークフローも、次の処理を実行します。

- JDK 25 と JDK 26 でアプリケーションをビルドし、テストする
- GraalVM JDK 25 でネイティブ実行ファイルをビルドする
- リポジトリシークレット `SONAR_TOKEN` が設定されている場合だけ SonarQube スキャンを実行する

ビルドシステムを選んだ後は、そのビルドシステムに対応するワークフローだけを残します。
これにより重複したビルドを避け、選択したビルド定義を唯一の正とできます。

## プロジェクト構成

```text
src/main/java/       アプリケーションのソースコード
src/main/resources/  アプリケーションの設定とリソース
src/test/java/       自動テスト
.github/workflows/   Maven と Gradle の CI 候補
```
