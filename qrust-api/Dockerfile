# 1단계: 빌드 전용 스테이지
FROM gradle:8.5-jdk21 AS build

WORKDIR /home/app

# Docker 캐시 효율성을 위해 빌드 설정 파일만 먼저 복사
COPY build.gradle settings.gradle gradle.properties ./
COPY gradle ./gradle
COPY qrust-common/build.gradle ./qrust-common/
COPY qrust-domain/build.gradle ./qrust-domain/
COPY qrust-api/build.gradle ./qrust-api/

# 의존성 다운로드 (변경이 없을 경우 캐시된 레이어 사용)
# --no-daemon 옵션은 CI/CD 환경에서 권장
RUN gradle build --no-daemon -x test --build-cache || true

# 전체 소스 코드 복사
COPY . .

# 애플리케이션 빌드 (이전 단계에서 실패했더라도 재시도)
RUN gradle :qrust-api:build --no-daemon -x test


# 2단계: 실행 전용 스테이지
FROM openjdk:21-jre-slim

WORKDIR /app

# 보안 강화를 위해 non-root 유저 생성 및 사용
#RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
#USER appuser

# 빌드 스테이지에서 생성된 JAR 파일 복사
COPY --from=build /home/app/qrust-api/build/libs/*.jar app.jar

# 포트 노출 (문서화 목적)
EXPOSE 8080

# 애플리케이션 실행
ENTRYPOINT ["java", "-jar", "app.jar"]