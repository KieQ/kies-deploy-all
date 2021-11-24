FROM kiesun/kieq_build:latest AS front_builder

WORKDIR /root

RUN git clone https://github.com/KieQ/kies-code-frontend.git

WORKDIR /root/kies-code-frontend

RUN sh script/build.sh

FROM kiesun/kieq_build:latest AS back_builder

WORKDIR /root

RUN git clone https://github.com/KieQ/kies-code-backend.git

WORKDIR /root/kies-code-backend

RUN sh script/build.sh

FROM nginx:latest

COPY --from=front_builder /root/kies-code-frontend/app/dist/ /usr/share/nginx/html

COPY nginx.conf /etc/nginx/nginx.conf

COPY default.conf.template /etc/nginx/conf.d/default.conf.template

COPY --from=back_builder /root/kies-code-backend/kies /root

COPY --from=back_builder /lib/x86_64-linux-gnu/libasan.so.5 /lib/x86_64-linux-gnu

CMD /root/kies& /bin/bash -c "envsubst '\$PORT' < /etc/nginx/conf.d/default.conf.template > /etc/nginx/conf.d/default.conf" && nginx -g 'daemon off;'
