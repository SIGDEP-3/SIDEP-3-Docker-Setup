# syntax=docker/dockerfile:1

### Dev stage
FROM openmrs/openmrs-core:dev as dev
WORKDIR /openmrs_distro

ARG MVN_ARGS_SETTINGS="-s /usr/share/maven/ref/settings-docker.xml -U"
ARG MVN_ARGS="clean install"

# Copy build files
COPY pom.xml ./
COPY distro ./distro/

# Build the distro
RUN --mount=type=secret,id=m2settings,target=/root/.m2/settings.xml mvn $MVN_ARGS_SETTINGS $MVN_ARGS

RUN cp /openmrs_distro/distro/target/sdk-distro/web/openmrs.war /openmrs/distribution/openmrs_core/

RUN cp /openmrs_distro/distro/target/sdk-distro/web/openmrs-distro.properties /openmrs/distribution/
RUN cp -R /openmrs_distro/distro/target/sdk-distro/web/owa /openmrs/distribution/openmrs_owas

# Since our internal maven repo isn't ready, let's use our locally managed omods as a pov
RUN cp -R /openmrs_distro/distro/target/sdk-distro/web/modules /openmrs/distribution/openmrs_modules

RUN cp -R ./distro/configuration /openmrs/distribution/openmrs_config/

# Clean up after copying needed artifacts
RUN mvn clean $MVN_ARGS_SETTINGS
