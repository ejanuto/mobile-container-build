FROM ubuntu:18.04
LABEL maintainer="janusz.tomanek@symphony.com"
WORKDIR /root

RUN apt-get update && apt-get -y install openssl maven vim openjdk-8-jdk wget unzip cpu-checker
RUN update-java-alternatives -s java-1.8.0-openjdk-amd64
RUN mkdir -p ~/.m2
COPY src/settings.xml ~/.m2/settings.xlm
COPY src/release.jks release.jks

#install latest androidSDK
RUN wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
RUN unzip sdk-tools-linux-4333796.zip
RUN mkdir ~/android-sdk
RUN mv tools ~/android-sdk/tools
RUN rm -f ~/sdk-tools-linux-4333796.zip

#add Andorid SDK env variables
RUN echo "# Export the Android SDK path" >> ~/.bashrc
RUN echo "export ANDROID_HOME=$HOME/android-sdk" >> ~/.bashrc
RUN echo "export PATH=\$PATH:\$ANDROID_HOME/tools/bin" >> ~/.bashrc
RUN echo "export PATH=\$PATH:\$ANDROID_HOME/platform-tools" >> ~/.bashrc

RUN echo 'y' | /root/android-sdk/tools/bin/sdkmanager "system-images;android-23;google_apis;x86_64" "system-images;android-25;google_apis;armeabi-v7a" "tools" "platform-tools" "platforms;android-25" "build-tools;25.0.2" "emulator"
RUN echo 'no' | avdmanager create avd -n Nexus5  -k "system-images;android-25;google_apis;armeabi-v7a" -c 200M --device "Nexus 5X" --force
