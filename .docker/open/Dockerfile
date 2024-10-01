FROM openjdk:17-jdk

ENV GRADLE_VERSION 7.5.1

WORKDIR /usr/local

# Cập nhật và cài đặt các gói cần thiết
RUN apt-get update \
    && apt-get install -y nodejs npm curl unzip \
    && rm -rf /var/lib/apt/lists/*

# Tải và cài đặt Gradle từ nguồn chính thức
RUN curl -fsSL https://downloads.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle.zip \
    && unzip gradle.zip \
    && rm gradle.zip

# Cập nhật PATH để bao gồm Gradle
ENV PATH "$PATH:/usr/local/gradle-${GRADLE_VERSION}/bin/"

WORKDIR /app

# Thiết lập biến môi trường cho Spring
ENV SPRING_PROFILE test

# Lệnh CMD để chạy ứng dụng
CMD echo "==============> Current environment: ${SPRING_PROFILE}" \
    && rm -f -r ./api/open/build \
    && gradle :api-open:build \
    && cd ./api/open/build/libs \
    && java -jar api-open-1.0-SNAPSHOT.jar --spring.profiles.active=${SPRING_PROFILE}
