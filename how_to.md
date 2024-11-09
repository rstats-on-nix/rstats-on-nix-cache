Steps:

- Go to digital ocean and set up a space. Then set up keys (in the API page). Copy the public and secret keys.
- Get `awscli` and `s3cmd`: `nix-shell -p awscl s3cmd`
- Configure: `aws configure --profile digitalocean`
- Set up `s3cmd` using `s3cmd --configure` by follow instructions here: https://docs.digitalocean.com/products/spaces/reference/s3cmd/
- Learn about common `s3cmd` here: https://docs.digitalocean.com/products/spaces/reference/s3cmd-usage/ and here: https://s3tools.org/usage
- Set up the bucket policy for unauthenticated reads: https://nix.dev/manual/nix/2.23/store/types/s3-binary-cache-store by first saving the contents into `policy.json` and then calling `s3cmd setpolicy policy.json s3://your-bucket-here`
- Then set up the bucket policy for authenticated puts from the same page as before: `s3cmd setpolicy policy_puts.json s3://your-bucket-here`
- Check if policy is set correctly: `echo -e $(aws s3api get-bucket-policy --bucket rstats-on-nix-cache --profile digitalocean --endpoint-url https://fra1.digitaloceanspaces.com)`
- Build the lolhello package from: https://fzakaria.com/2020/07/15/setting-up-a-nix-s3-binary-cache.html
- Push the lolhello package: `nix copy $(nix-store --query --requisites --include-outputs $(nix-store --query --deriver ./result)) --to 's3://rstats-on-nix-cache?profile=digitalocean&scheme=https&endpoint=fra1.digitaloceanspaces.com' --option narinfo-cache-positive-ttl 0`
