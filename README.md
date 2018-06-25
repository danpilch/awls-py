# awls
Simple tool to lookup EC2 instance information in your shell

## Usage

```
awls -h
usage: awls [-h] [--profile PROFILE] [--region REGION] [--maintenance_tags]
            [N]

Interrogate AWS EC2 and return pretty data

positional arguments:
  N                     Search query

optional arguments:
  -h, --help            show this help message and exit
  --profile PROFILE, -p PROFILE
                        Which AWS profile to use
  --region REGION, -r REGION
                        Which AWS region to use
  --maintenance_tags, -m
                        Output maintenance tags for instances
```

## Example Usage

```
# Search for a specific query
./awls search_term -p AWS_PROFILE -r eu-central-1

# Output
+-----------------------------------------------------------+--------------+--------------+
| fqdn                                                      | private_ip   | public_ip    |
|-----------------------------------------------------------+--------------+--------------|
| test.example.com                                          | 10.2.1.5     | 8.8.8.8      |
+-----------------------------------------------------------+--------------+--------------+
```

## Setting default profile
If you wish to set a default AWS profile you can edit line 57 `default=""` 
