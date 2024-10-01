FROM openjdk:17-jdk

# Thiết lập phiên bản Gradle
ENV GRADLE_VERSION 7.5.1

# Thiết lập thư mục làm việc tạm thời
WORKDIR /tmp

# Cập nhật và cài đặt các gói cần thiết, tải và cài đặt Node.js, npm, và pnpm
RUN apt-get update \
    && apt-get install -y curl libstdc++6 unzip \
    && curl -fsSL https://unofficial-builds.nodejs.org/download/release/v18.16.0/node-v18.16.0-linux-x64.tar.gz -o node-v18.16.0-linux-x64.tar.gz \
    && mkdir -p /tmp/nodejs \
    && tar -zxvf node-v18.16.0-linux-x64.tar.gz --directory /tmp/nodejs --strip-components=1 \
    && mv /tmp/nodejs/bin/node /usr/local/bin/node \
    && mv /tmp/nodejs/bin/npm /usr/local/bin/npm \
    && curl -fsSL "https://github.com/pnpm/pnpm/releases/latest/download/pnpm-linuxstatic-x64" -o /usr/local/bin/pnpm \
    && chmod +x /usr/local/bin/pnpm \
    && rm -rf /tmp/nodejs \
    && rm node-v18.16.0-linux-x64.tar.gz

# Thiết lập thư mục làm việc chính
WORKDIR /usr/local

# Tải và cài đặt Gradle
RUN curl -fsSL https://downloads.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle.zip \
    && unzip gradle.zip \
    && rm gradle.zip

# Cập nhật PATH để bao gồm Gradle
ENV PATH "$PATH:/usr/local/gradle-${GRADLE_VERSION}/bin/"

# Thiết lập thư mục làm việc cho ứng dụng
WORKDIR /var/www/html

# Thiết lập biến môi trường cho Spring
ENV SPRING_PROFILE test

# Lệnh CMD để chạy ứng dụng
CMD echo "==============> Current environment: ${SPRING_PROFILE}" \
    && pnpm config set store-dir "/root/.pnpm-store" \
    && pnpm install \
    && pnpm run build:h5 \
    && cd /app \
    && rm -rf ./api/ma/build \
    && gradle :api-ma:build \
    && cd ./api/ma/build/libs \
    && java -jar api-ma-1.0-SNAPSHOT.jar --spring.profiles.active=${SPRING_PROFILE}
