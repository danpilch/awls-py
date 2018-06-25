#!/usr/bin/env python
# Maintainer: Dan Pilch <dan@ctrlengineering.com>

import boto3
import boto3.session
from tabulate import tabulate
import argparse


class AWSInventoryTable(object):
    def __init__(self, aws_profile, aws_region):
        self.session = boto3.session.Session(profile_name=aws_profile, region_name=aws_region)

    def generate_table(self, query, maintenance_tags):
        ec2 = self.session.resource('ec2')
        instances = ec2.instances.filter()
        tbl_list = []

        for instance in instances:
            # Using a flag so we don't add data to the list we don't actually want
            found = False
            if instance.tags is not None:
                for tag in instance.tags:
                    if tag['Key'] == "Name":
                        # if a query was passed, try to find instances that match
                        if query:
                            if query in tag['Value']:
                                aws_name = tag['Value']
                                aws_private_ip = instance.private_ip_address
                                aws_public_ip = instance.public_ip_address
                                found = True
                            else:
                                found = False
                        else:
                            aws_name = tag['Value']
                            aws_private_ip = instance.private_ip_address
                            aws_public_ip = instance.public_ip_address
                            found = True

                    if tag['Key'] == "Maintenance":
                        aws_maintenance_tags = tag['Value']

            if found:
                if maintenance_tags:
                    tbl_list.append([aws_name, aws_private_ip, aws_public_ip, aws_maintenance_tags])
                    headers = ["fqdn", "private_ip", "public_ip", "maintenance_tags"]
                else:
                    tbl_list.append([aws_name, aws_private_ip, aws_public_ip])
                    headers = ["fqdn", "private_ip", "public_ip"]

        # Return pretty data
        print(tabulate(tbl_list, headers=headers, tablefmt="psql"))


def main():
    parser = argparse.ArgumentParser(description="Interrogate AWS EC2 and return pretty data")
    parser.add_argument("--profile", "-p", help="Which AWS profile to use", default="")
    parser.add_argument("--region", "-r", help="Which AWS region to use", default="ap-southeast-2")
    parser.add_argument("--maintenance_tags", "-m", help="Output maintenance tags for instances", action="store_true", default=False)
    parser.add_argument("query", metavar="N", nargs='?', help="Search query", default=None)
    args = parser.parse_args()

    run = AWSInventoryTable(args.profile, args.region)
    run.generate_table(args.query, args.maintenance_tags)


if __name__ == "__main__":
    main()
