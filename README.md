# Praktikum Mobile Programming

1. Buat Keystore untuk Penandatanganan Aplikasi:
```bash
keytool -genkey -v -keystore ~/my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
```

2. Tambahkan Keystore dan Secrets ke Proyek Flutter:
   a. Buat file `key.properties` di root proyek:
   ```properties
   storePassword=<your-keystore-password>
   keyPassword=<your-key-password>
   keyAlias=my-key-alias
   storeFile=<path-to-your-keystore>
   ```
   b. Edit file `build.gradle` di `android/app`:
   ```groovy
   def keystoreProperties = new Properties()
   def keystoreFile = rootProject.file('key.properties')
   if (keystoreFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystoreFile))
   }

   android {
       ...
       signingConfigs {
           release {
               keyAlias keystoreProperties['keyAlias']
               keyPassword keystoreProperties['keyPassword']
               storeFile file(keystoreProperties['storeFile'])
               storePassword keystoreProperties['storePassword']
           }
       }
       buildTypes {
           release {
               signingConfig signingConfigs.release
           }
       }
   }
   ```

3. Build Android App Bundle (AAB):
```bash
flutter build appbundle
```

4. Unggah Proyek ke GitHub:
```bash
git init
git add .
git commit -m 'Initial commit'
git remote add origin <repository_url>
git push -u origin master
```