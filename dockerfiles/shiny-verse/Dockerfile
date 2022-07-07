FROM rocker/shiny-verse:4.1.2

COPY shiny-server.conf /etc/shiny-server/shiny-server.conf
RUN mkdir /srv/code/
RUN chown -R shiny /var/lib/shiny-server/ && chown -R shiny /srv/code/

# OpenShift gives a random uid for the user and some programs try to find a username from the /etc/passwd.
# Let user to fix it, but obviously this shouldn't be run outside OpenShift
RUN chmod ug+rw /etc/passwd
COPY fix-username.sh /usr/bin/fix-username.sh
COPY shiny-server.sh /usr/bin/shiny-server.sh
RUN chmod a+rx /usr/bin/shiny-server.sh

# Make sure the directory for individual app logs exists and is usable
RUN chmod -R a+rwX /var/log/shiny-server
RUN chmod -R a+rwX /var/lib/shiny-server

#Custom
RUN cp -r /srv/shiny-server/sample-apps/hello /srv/code/

CMD /usr/bin/shiny-server.sh
