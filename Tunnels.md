## Document DB dev
### Tunnel Command
``` bash
ssh -L 27017:dev-shared-db.cluster-cb7o2jhnuupx.us-east-1.docdb.amazonaws.com:27017 -N jumpbox@34.239.231.116
```
### Connection URL
`mongodb://root:jeMJdWw8xtl31WCN@localhost:27017/?replicaSet=rs0&readPreference=secondaryPreferred&retryWrites=false&directConnection=true`

## Redis Cluster Dev

#### Main endpoint
```bash
ssh -L 6379:clustercfg.redis-cluster-1-dev.u7ajbh.use1.cache.amazonaws.com:6379 -N jumpbox@34.239.231.116
```

#### Cluster endpoint
- Need to set this up so that when the redis redirect, we can redirect it to the correct port

```bash
ssh -N -L 7000:redis-cluster-1-dev-0001-001.redis-cluster-1-dev.u7ajbh.use1.cache.amazonaws.com:6379 jumpbox@34.239.231.116
```

```bash
ssh -N -L 7001:redis-cluster-1-dev-0001-002.redis-cluster-1-dev.u7ajbh.use1.cache.amazonaws.com:6379 jumpbox@34.239.231.116
```
## Get AWS secret
``` bash 
aws secretsmanager get-secret-value --secret-id dev/app/bodimatch/API_KEY_ENCRYPTION_PASSPHARSE
```
