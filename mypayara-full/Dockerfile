FROM payara/server-full:5.183

ARG POSTGRES_PASSWORD
ARG POSTGRES_USER
ARG POSTGRES_PORT
ARG POSTGRES_HOST
ARG POSTGRES_DATABASE
ARG POSTGRES_POOL_NAME

ARG APP_VERSION
ARG APP_SHORT_NAME
ARG PROJECT_DOWNLOAD_PATH

RUN ${PAYARA_PATH}/generate_deploy_commands.sh
RUN ${DEPLOY_COMMANDS}

ENTRYPOINT echo 'set-hazelcast-configuration --enabled=true --dynamic=true' >> mycommands.asadmin && \
  echo 'create-jdbc-connection-pool --datasourceclassname org.postgresql.ds.PGConnectionPoolDataSource --restype javax.sql.ConnectionPoolDataSource --property portNumber=${POSTGRES_PORT}:password=${POSTGRES_PASSWORD}:user=${POSTGRES_USER}:serverName=${POSTGRES_HOST}:databaseName=${POSTGRES_DATABASE} ${POSTGRES_POOL_NAME}' > mycommands.asadmin && \
  echo 'create-jdbc-resource --connectionpoolid ${POSTGRES_POOL_NAME} jdbc/${POSTGRES_POOL_NAME}' >> mycommands.asadmin && \
  wget https://jdbc.postgresql.org/download/postgresql-9.4.1212.jar -O /opt/payara5/glassfish/domains/domain1/lib/postgresql.jar && \
  wget ${PROJECT_DOWNLOAD_PATH}/${APP_SHORT_NAME}/${APP_VERSION}/${APP_SHORT_NAME}-${APP_VERSION}.war -O /opt/payara5/glassfish/domains/domain1/autodeploy/app.war && \
  /opt/payara5/bin/asadmin start-domain -v --postbootcommandfile mycommands.asadmin ${PAYARA_DOMAIN}
