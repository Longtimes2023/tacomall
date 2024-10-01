FROM openjdk:17-jdk

# Thiết lập biến môi trường cho Gradle
ENV GRADLE_VERSION 7.5.1

# Thiết lập thư mục làm việc
WORKDIR /usr/local

# Cập nhật và cài đặt các gói cần thiết
RUN apt-get update \
    && apt-get install -y curl unzip bash \
    && curl -fsSL https://downloads.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle.zip \
    && unzip gradle.zip \
    && rm gradle.zip

# Thiết lập PATH cho Gradle
ENV PATH "$PATH:/usr/local/gradle-${GRADLE_VERSION}/bin/"

# Cài đặt Node.js và npm từ nguồn chính thức
RUN curl -fsSL https://unofficial-builds.nodejs.org/download/release/v18.16.0/node-v18.16.0-linux-x64.tar.gz -o node.tar.gz \
    && mkdir -p /usr/local/nodejs \
    && tar -zxvf node.tar.gz --directory /usr/local/nodejs --strip-components=1 \
    && ln -s /usr/local/nodejs/bin/node /usr/local/bin/node \
    && ln -s /usr/local/nodejs/bin/npm /usr/local/bin/npm \
    && rm node.tar.gz

# Thiết lập thư mục làm việc cho ứng dụng
WORKDIR /var/www/html

# Thiết lập biến môi trường cho Spring
ENV SPRING_PROFILE test

# Lệnh CMD để chạy ứng dụng
CMD echo "==============> Current environment: ${SPRING_PROFILE}" \
    && npm install \
    && npm run build \
    && cd /app \
    && rm -rf ./api/admin/build \
    && gradle :api-admin:build \
    && cd ./api/admin/build/libs \
    && java -jar api-admin-1.0-SNAPSHOT.jar --spring.profiles.active=${SPRING_PROFILE}
