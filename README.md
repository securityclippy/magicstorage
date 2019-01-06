#### S3Storage

S3Storage is a storage module to make use of amazon's s3 object storage as a storage interface for CertMagic.  S3Storage
is an implementation of the Storage interface which can be used to back services behind a load balancer without
needing to expose the servers themselves to the internet.  It is expected that users needing this storage will
have the requisite knowledge to understand when it is necessary to use this Storage Type in lieu of the default
Filestorage type.  Because this storage type is typically used behind a load balancer. it is most commonly used in
conjunction with the __DNS-01__ lego challenge.

The `magicstorage.NewS3Storage(bucket, region string)` function requires the name of the s3 bucket to be used as well as
the region for the s3 bucket.  __NOTE:__ Even though s3 buckets are "global", this is still a region associated with buckets.
do not pass an empty string

The `NewS3Storage()` function will automatically use credentials from ENV vars, `~/.aws/credentials` files and any assumed roles.
It should not be necessary to provide any explicit credentials.

Used with the certmagic HTTPS command and a dns provider:
```
dnsProvider, err := route53.NewDNSProvider()
if err != nil {
    return err
}

certmagic.DNSProvider = dnsProvider
certmagic.DefaultStorage = magicstorage.NewS3Storage("my-example-s3-bucket", "example-aws-region")

//Then use as normal

certmagic.HTTPS([]string{"example.com"}, handler)
```