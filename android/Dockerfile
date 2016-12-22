FROM irfanah/appium-base

ENV ANDROID_SDK_URL="https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz" \
    ANDROID_BUILD_TOOLS_VERSION=25.0.0 \
    ANDROID_APIS="android-24,android-23,android-22,android-21,android-19" \
    ANDROID_HOME="/opt/android-sdk-linux"

ENV PATH $PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$ANDROID_HOME/build-tools/$ANDROID_BUILD_TOOLS_VERSION

#================================================================
# Includes Dependencies
#================================================================
RUN dpkg --add-architecture i386 && \
          apt-get -qq update && \
          apt-get -qq install -y curl libncurses5:i386 libstdc++6:i386 zlib1g:i386  && \

#================================================================
# Includes Android SDK
#================================================================
    curl -sL ${ANDROID_SDK_URL} | tar xz -C /opt && \
    echo y | android update sdk -a -u -t platform-tools,${ANDROID_APIS},build-tools-${ANDROID_BUILD_TOOLS_VERSION},sys-img-armeabi-v7a-android-24 && \
    chmod a+x -R $ANDROID_HOME && \
    chown -R root:root $ANDROID_HOME && \

    # Clean up
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    apt-get autoremove --purge -y && \
    apt-get clean

#================================================================
# Creates Nexus 5 Android-24 Emulator by default
#================================================================
RUN echo "no" | android create avd \
                --force \
                --device "Nexus 5" \
                --name "Nexus" \
                --target android-24 \
                --abi armeabi-v7a \
                --skin WVGA800

#=======================================
# Expose default port of adb
#=======================================
EXPOSE 5554

RUN mkdir -p /opt/android
WORKDIR /opt/android

CMD emulator64-arm -avd Nexus -no-boot-anim -no-window -noaudio -gpu off -port 5554
