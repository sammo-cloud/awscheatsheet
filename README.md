# awscheatsheet

aws configure

for region in $(aws ec2 describe-regions --query "Regions[*].RegionName" --output text); do
  echo "Region: $region"
  aws ec2 describe-instances --region $region --query "Reservations[*].Instances[*].[InstanceId, State.Name]" --output table
done

aws ec2 describe-instances --instance-ids <instance id> --region <region id>

TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/

aws ec2 describe-network-acls

server {
    listen 443 ssl;
    server_name api.example.com;

    location /api/ {
        # Allow requests only from a specific origin
        set $cors_origin "https://trusted-frontend.com";

        if ($http_origin = "https://trusted-frontend.com") {
            add_header Access-Control-Allow-Origin $cors_origin;
            add_header Access-Control-Allow-Credentials true;
            add_header Access-Control-Allow-Methods "GET, POST, OPTIONS";
            add_header Access-Control-Allow-Headers "Authorization, Content-Type";
        }

        # Handle preflight requests
        if ($request_method = 'OPTIONS') {
            return 204;
        }

        # Proxy requests to the backend
        proxy_pass http://backend_server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
