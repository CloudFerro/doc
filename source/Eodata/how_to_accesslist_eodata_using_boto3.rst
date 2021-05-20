How to access/list EO DATA using boto3?
=======================================

.. toctree::
  :maxdepth: 3
  :caption: Elasticity

.. warning::
  To use boto3 your virtual machine has to be initialized in project with eo data.
  We strongly recommend using virtualenv for isolating python packages. 

 

If virtualenv is activated:

  ``$ pip install boto3``

Or if we install package globally:

  ``$ sudo pip install boto3``

 

Two examples for listing products:

 

Client:

    low-level service access
    generated from service description
    exposes botocore client to the developer
    typically maps 1:1 with the service API
    snake-cased method names (e.g. ListBuckets API => list_buckets method)


 .. code-block:: language

   
    import boto3

    access_key='anystring'
    secret_key='anystring'
  
    host='http://data.cloudferro.com'

    s3=boto3.client('s3',aws_access_key_id=access_key,
    aws_secret_access_key=secret_key, endpoint_url=host,)

    for i in s3.list_objects(Delimiter='/', Bucket="DIAS", Prefix='Sentinel-2/MSI/L1C/2020/01/08/',MaxKeys=30000)['CommonPrefixes']:
           print(i)

Resource:

    higher-level, object-oriented API
    generated from resource description
    uses identifiers and attributes
    has actions (operations on resources)
    exposes subresources and collections

 

import boto3

access_key='anystring'
secret_key='anystring'

host='http://data.cloudferro.com'


s3=boto3.resource('s3',aws_access_key_id=access_key,
aws_secret_access_key=secret_key, endpoint_url=host,)

bucket=s3.Bucket('DIAS')

for obj in bucket.objects.filter(Prefix='Sentinel-2/MSI/L1C/2016/02/03/S2A_OPER_PRD_MSIL1C_PDMC_20160204T043037_R085_V20160203T203520_20160203T203520.SAFE/'):
     print('{0}:{1}'.format(bucket.name, obj.key))

Save your file with .py extension and run with the "python [filename.py]" command in your terminal.
