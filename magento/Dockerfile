FROM java:7
MAINTAINER David Nunez <dnunez24@gmail.com>

ENV SOLR_VERSION 3.6.2
ENV SOLR apache-solr-$SOLR_VERSION
ENV SOLR_HOME /opt/solr
ENV SOLR_USER solr
ENV SOLR_PORT 8983

WORKDIR /usr/src
RUN wget http://archive.apache.org/dist/lucene/solr/$SOLR_VERSION/$SOLR.tgz \
  && wget http://archive.apache.org/dist/lucene/solr/$SOLR_VERSION/$SOLR.tgz.asc \
  && wget http://archive.apache.org/dist/lucene/solr/$SOLR_VERSION/KEYS \
  && gpg --import KEYS \
  && gpg --verify $SOLR.tgz.asc $SOLR.tgz \
  && tar -xzf $SOLR.tgz \
  && cp -R $SOLR/example $SOLR_HOME \
  && rm -rf $SOLR \
  && rm $SOLR.tgz \
  && rm $SOLR.tgz.asc \
  && rm KEYS \
  && groupadd -r $SOLR_USER \
  && useradd -r -d $SOLR_HOME -s /bin/false -g $SOLR_USER $SOLR_USER
COPY conf/* $SOLR_HOME/conf/
COPY jetty-logging.xml $SOLR_HOME/etc/jetty-logging.xml
COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chown -R $SOLR_USER:$SOLR_USER $SOLR_HOME

EXPOSE $SOLR_PORT
USER $SOLR_USER
WORKDIR $SOLR_HOME
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["solr", "-Xms512m", "-Xmx512m"]
