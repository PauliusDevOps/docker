docker volume create n8n_data

docker run -d \
 --name n8n \
 -p 5678:5678 \
 -e GENERIC_TIMEZONE=“Europe\Vilnius” \
 -e TZ=“Europe\Vilnius \
 -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
 -e N8N_RUNNERS_ENABLED=true \
 -e N8N_SECURE_COOKIE=false \
 -e N8N_EXPOSE_API=true \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n
