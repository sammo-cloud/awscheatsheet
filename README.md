# awscheatsheet

aws configure

for region in $(aws ec2 describe-regions --query "Regions[*].RegionName" --output text); do
  echo "Region: $region"
  aws ec2 describe-instances --region $region --query "Reservations[*].Instances[*].[InstanceId, State.Name]" --output table
done
