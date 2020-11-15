
# SPA FALLBACK TO INDEX 
RUN cat default.conf | sed "11i try_files \$uri /index.html;"