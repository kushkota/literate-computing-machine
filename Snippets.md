This document contains the commands combining curl with jq (Command-line JSON processor)

We are leveraging AWS IP address ranges .json file provided on AWS General Reference documentation to learn about these tools and various techniques.

## Verify JSON document
```bash
curl --head https://ip-ranges.amazonaws.com/ip-ranges.json -o /dev/null -s -w "\nContent-Type: \t%{content_type}\n\n"
```

## The document has IPv4 and IPv6 addresses, locating line number to parse data in subsequent request
```bash
curl https://ip-ranges.amazonaws.com/ip-ranges.json -s | grep -n prefixes
```

## This JSON file listed 22 AWS Services and bulk of the addresses belongs to service "Amazon" and "EC2" subsequently.
```bash
curl https://ip-ranges.amazonaws.com/ip-ranges.json -s | jq '.prefixes[].service' | sort | uniq -c | sort -rn | nl
```

## Displaying IPv4 raw strings rather than JSON texts (only first IP)
```bash
curl https://ip-ranges.amazonaws.com/ip-ranges.json -s | jq -r '.prefixes | .[].ip_prefix' | head -1
```
