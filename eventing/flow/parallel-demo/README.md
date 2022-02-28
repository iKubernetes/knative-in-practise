# Parallel


### Filter

kn service apply image-filter --image villardl/filter-nodejs:0.1 --env FILTER='event.type == "com.magedu.file.image"' -n event-demo
kn service apply text-filter --image villardl/filter-nodejs:0.1 --env FILTER='event.type == "com.magedu.file.text"' -n event-demo


### Subscriber

kn service apply para-appender-image --image ikubernetes/appender --env MESSAGE=" - filetype/Image" -n event-demo
kn service apply para-appender-text --image ikubernetes/appender --env MESSAGE=" - filetype/Text" -n event-demo


### Test

```bash
kubectl run client-$RANDOM --image=ikubernetes/admin-box:v1.2 --restart=Never --rm -it --command -- /bin/bash
```


curl -v "http://filetype-parallel-kn-parallel-kn-channel.event-demo.svc.cluster.local" \
-X POST \
-H "Ce-Id: 0" \
-H "Ce-Specversion: 1.0" \
-H "Ce-Type: com.magedu.file.image" \
-H "Ce-Source: Curl" \
-H "Content-Type: application/json" \
-d '{"message": "A uploaded file by a user"}'


curl -v "http://filetype-parallel-kn-parallel-kn-channel.event-demo.svc.cluster.local" \
-X POST \
-H "Ce-Id: 0" \
-H "Ce-Specversion: 1.0" \
-H "Ce-Type: com.magedu.file.text" \
-H "Ce-Source: Curl" \
-H "Content-Type: application/json" \
-d '{"message": "A uploaded file by a user"}'
