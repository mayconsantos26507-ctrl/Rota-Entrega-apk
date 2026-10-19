
name: Build Rota Express Moto

on:
  workflow_dispatch:
  push:
    branches: [main, master]

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - uses: subosito/flutter-action@v2
        with:
          channel: stable

      - name: Extract project
        run: unzip -o rota_express_moto_mvp.zip

      - name: Build APK
        working-directory: rota_express_moto
        run: |
          flutter create --platforms=android --org com.rotaexpress .
          flutter pub get
          flutter build apk --release

      - uses: actions/upload-artifact@v4
        with:
          name: rota-express-moto-apk
          path: rota_express_moto/build/app/outputs/flutter-apk/app-release.apk
