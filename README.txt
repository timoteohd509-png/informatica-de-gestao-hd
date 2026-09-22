name: Gerar APK - Informática de Gestão HD

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Baixar projeto
        uses: actions/checkout@v4

      - name: Configurar Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Configurar Android
        uses: android-actions/setup-android@v3

      - name: Instalar SDK
        run: |
          sdkmanager "platform-tools"
          sdkmanager "platforms;android-35"
          sdkmanager "build-tools;35.0.0"

      - name: Preparar projeto
        run: |
          unzip -o "Informática_de_Gestão_HD_GitHub_Telemóvel_COMPLETO.zip" -d projeto
          cp -r projeto/* .

      - name: Configurar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'

      - name: Gerar APK
        run: gradle assembleDebug

      - name: Guardar APK
        uses: actions/upload-artifact@v4
        with:
          name: Informática-de-Gestão-HD-APK
          path: '**/build/outputs/apk/debug/*.apk'
