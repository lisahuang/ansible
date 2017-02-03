Step by step create amazon instance

1. Replace the ACCESS_KEY_HERE and SECRET_KEY_HERE with your AWS access key and secret key, available from this page https://console.aws.amazon.com/iam/home?#/security_credential. 

2. Authorize test_sec_group with ssh access by following this page http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html.

3. Create a key-pair my_key_pair by following this page: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html.

4. save the follow as hello_amazon.py

pip install ansible
pip install boto

import boto.ec2

aws_access_key_id='accesskey',
aws_secret_access_key='secret'
conn = boto.ec2.connect_to_region("us-east-1", aws_access_key_id=aws_access_key_id,
	aws_secret_access_key=aws_secret_access_key)

ubuntu = "ami-0d729a60"
conn.run_instances(ubuntu, instance_type ="t2.micro", key_name='my_key_pair', security_group_ids='test_sec_group')

5. create instance with script. You can create manually or using GUI from this page http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html

python hello_amazon.py

This should create an ubuntu ec2 instance and launched it.

6. tag the instance with "test"

7. Test instance
7.1 test your instance with ping:
ping 52.204.85.73

7.2 test your instance with ssh:
ssh -i ../../my_key_pair.pem ubuntu@52.204.85.73

7.3 test with ansible ping:
ansible all -i hosts -m ping -u ubuntu
ansible all -i hosts -m ping --private-key ../../my_key_pair.pem -u ubuntu

$ more hosts
[amazon]
52.203.116.69

7.4 Add ansible.cfg file
$ more ansible.cfg
[defaults]
private_key_file=../../my_key_pair.pem

7.5 test with ansible ping
ansible -m ping tag_test -u ubuntu -vvv



reference: https://aws.amazon.com/blogs/apn/getting-started-with-ansible-and-dynamic-amazon-ec2-inventory-management/