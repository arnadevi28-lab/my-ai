name: Build My AI APK

on:
  workflow_dispatch:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          channel: stable

      - name: Extract ZIP
        run: unzip My_AI_Flutter_Project.zip

      - name: Create Android files
        working-directory: my_ai_flutter
        run: flutter create . --platforms=android --project-name=my_ai

      - name: Get packages
        working-directory: my_ai_flutter
        run: flutter pub get

      - name: Build APK
        working-directory: my_ai_flutter
        run: flutter build apk --debug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: my-ai-debug-apk
          path: my_ai_flutter/build/app/outputs/flutter-apk/app-debug.apk
          
