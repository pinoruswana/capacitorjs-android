# Proyek Baru Aplikasi Android dengan Capacitor 4.x

Panduan membuat proyek baru aplikasi Android dari nol di Linux, menggunakan Capacitor JS versi 4.x, Vite sebagai bundler, dan Android Tools CLI. Tanpa Android Studio. 

**Catatan:** Tools yang perlu diinstall bisa dilihat pada [bagian ini](#panduan-install-tools).
## Siapkan folder proyek
Buatlah sebuah folder untuk proyek, misalnya:
``` bash
mkdir ProyekApp
cd ProyekApp
```

## Buat file: /package.json
Contoh file ``package.json``, dependency bisa disesuaikan dengan kebutuhan.
``` json
{
  "name": "proyek-app",
  "version": "1.0.0",
  "type": "module",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "sync": "npm run build && npx cap sync",
    "android": "npm run sync && npx cap run android"
  },
  "dependencies": {
    "@capacitor-community/background-geolocation": "^1.2.26",
    "@capacitor/android": "^4.8.2",
    "@capacitor/core": "^4.8.2",
    "@capacitor/filesystem": "^4.1.5",
    "@capacitor/geolocation": "^4.1.0",
    "@capacitor/share": "^4.1.1",
    "jspdf": "^3.0.2",
    "jspdf-autotable": "^5.0.2"
  },
  "devDependencies": {
    "@capacitor/cli": "^4.8.2",
    "@capacitor/assets": "^3.0.5",
    "vite": "^7.1.2"
  }
}
```
## Buat file: /capacitor.config.json
Contoh file config, jangan lupa sesuaikan ``appId``
``` json
{
  "appId": "id.web.solidsoft.proyekapp",
  "appName": "Proyek App",
  "webDir": "dist",
  "bundledWebRuntime": false
}
```
## Buat file: /vite.config.js
``` javascript
import { defineConfig } from 'vite';

// Konfigurasi Vite untuk Capacitor
export default defineConfig({
  root: '.', // project root
  base: './', // penting biar path asset cocok di Android/iOS
  build: {
    outDir: 'dist', // wajib sama dengan capacitor.config.json
    emptyOutDir: true,
  },
  server: {
    port: 5173,
    open: true,
  },
});
```
## Buat file: /index.html
Contoh file ``index.html`` sebagai halaman awal aplikasi.
``` html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Judul Halaman</title>
  </head>
  <body>
    <h1>Hello Bos 👋</h1>
    <p>Ini jalan tanpa main.js</p>
    <script>
      console.log("Halo dari inline script!");
    </script>
  </body>
</html>
```
## Install Paket NPM
Install paket NPM sesuai yang telah di definikan pada ``package.json`` sebelumnya.
``` bash
npm install --legacy-peer-deps
```
## Inisialisasi Capacitor
Jalankan inisialisasi Capacitor.
``` bash
npx cap init
```
## Tambahkan platform Android
``` bash
npx cap add android
```
## Compile proyek
Edit source code -- misalnya: ``index.html``, dst. -- Kemudian jalankan perintah ini untuk meng-compile
``` bash
npm run android
``` 


# Panduan Install Tools Android
### 1. Install Java (OpenJDK 17)
``` bash
sudo apt update
sudo apt install -y openjdk-17-jdk
```
### 2. Install Android CLI Tools
``` bash
# bikin folder untuk Android SDK
mkdir -p ~/Android/cmdline-tools
cd ~/Android/cmdline-tools

# download command line tools (Linux)
wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip

# extract ke folder "latest"
unzip commandlinetools-linux-*_latest.zip
mkdir -p latest
mv cmdline-tools/* latest/
rm -rf cmdline-tools commandlinetools-linux-*_latest.zip
```
### 3. Install SDK/Platform Tools
``` bash
sdkmanager --install "platform-tools" "platforms;android-34" "emulator"
```
### 4. Install Gradle
``` bash
wget https://services.gradle.org/distributions/gradle-8.11.1-bin.zip -P /tmp
sudo unzip -d /opt/gradle /tmp/gradle-8.11.1-bin.zip
```
### 5. Atur Path pada Shell Environment
Contoh config shell, silahkan sesuaikan path.
#### Fish
``` bash
# File: ~/.config/fish/config.fish

eval (ssh-agent -c) 

if status is-interactive
    set -Ux JAVA_HOME /usr/lib/jvm/java-17-openjdk-amd64
    set -Ux ANDROID_AVD_HOME /home/me/.android/avd
    set -Ux ANDROID_HOME /media/me/5C20609820607AC2/Android
    set -Ux GRADLE_HOME /opt/gradle/gradle-8.11.1
    
    set -Ux fish_user_paths $ANDROID_HOME/cmdline-tools/latest/bin $fish_user_paths
    set -Ux fish_user_paths $ANDROID_HOME/platform-tools  $fish_user_paths
    set -Ux fish_user_paths $ANDROID_HOME/emulator $fish_user_paths
    set -Ux fish_user_paths $GRADLE_HOME/bin $fish_user_paths
    set -Ux fish_user_paths $JAVA_HOME/bin $fish_user_paths
end
```
#### Bash / Zsh
``` bash
# File: ~/.bashrc

# Jalankan ssh-agent
eval "$(ssh-agent -s)"

# Kalau shell interaktif
if [[ $- == *i* ]]; then
    export JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"
    export ANDROID_AVD_HOME="$HOME/.android/avd"
    export ANDROID_HOME="/media/me/5C20609820607AC2/Android"
    export GRADLE_HOME="/opt/gradle/gradle-8.11.1"

    export PATH="$ANDROID_HOME/cmdline-tools/latest/bin:$PATH"
    export PATH="$ANDROID_HOME/platform-tools:$PATH"
    export PATH="$ANDROID_HOME/emulator:$PATH"
    export PATH="$GRADLE_HOME/bin:$PATH"
    export PATH="$JAVA_HOME/bin:$PATH"
fi
```
### 6. Test instalasi
``` bash
java -version
gradle -v
sdkmanager --list
```

