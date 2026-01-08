# Regions:
- A Region is a geographically distinct area with a collection of data centers. 
- Each region operates independently, one failure region don't affect another one.
- Typically has multiple AZs

# Availability Zones (AZs):
 - An Availability Zone is a distinct physical location within a region. 
 - Each AZ consists of one or more data centers.
 - up to about 60 miles for fault isolation

 # Hight avaliability Amazon
- Use 2+ AZs
- Use ALB
- Auto Scaling
- Resilient storage (RDS, S3, DynamoDB)