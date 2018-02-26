---
layout:     post
title:      HPQC auto update tool
subtitle:    "little tool develop"
date:       2018-02-02
author:     Tong
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - Python
    - tool
---



For tester, there are various test results summary tools, this time I will introduce how to update test results in HPQC use a auto script. 

```python
# -*- coding: UTF-8 -*-
'''
function: This script is used for updating test result in HPQC\n
author : Tong\n
version: ver1\n
date: 2018/01/29
'''

import os
import requests
import logging
import copy
from xml.etree import ElementTree

logging.basicConfig(filename='HPQC.log', filemode='a',
                    format='%(asctime)s,%(msecs)d %(name)s %(levelname)s %(message)s', level=logging.DEBUG)
logging.debug("---------------------- update HPQC ----------------------\n")
logging.info('The current path' + os.path.abspath('.'))


class HpUpdate(object):
    def __init__(self, QC_owner, QC_domain, QC_project):
        self.QC_owner = QC_owner
        self.QC_domain = QC_domain
        self.QC_project = QC_project

    @staticmethod
    def get_instance(get_instance_url, get_headers):
        get_instance = requests.get(url=get_instance_url, headers=get_headers, verify=False)
        instance_tree = ElementTree.fromstring(get_instance.content)
        for elem in instance_tree.findall('Entity/Fields/Field'):  # get instance ID
            if (elem.attrib['Name'] == 'id'):
                test_instance = elem[0].text
                test_instances.append(test_instance)
            elif (elem.attrib['Name'] == 'status'):
                test_status = elem[0].text
                test_statuses.append(test_status)
            elif (elem.attrib['Name'] == 'test-id'):
                test_id = elem[0].text
                test_ids.append(test_id)
            else:
                pass
        return test_instances, test_statuses, test_ids

    @staticmethod
    def get_status(get_filed_url, get_headers):
        get_field = requests.get(url=get_filed_url, headers=get_headers, verify=False)
        field_tree = ElementTree.fromstring(get_field.content)
        for elem in field_tree.findall('List'):
            name = elem.find('Name')
            if name.text == 'Status':
                items = elem.findall('Items/Item')
                for item in items:
                    value = item.get('value')
                    all_status.append(value)
                break
        return all_status

    def post_method(self, test_instances, post_headers, status, build):
        for test_instance in test_instances:
            data = 'tester={}&testInstanceID={}&status={}&build={}'.format(self.QC_owner, test_instance,
                                                                                           status, build)
            post_instance_url = 'https://quality-api.eng.vmware.com:8443/QCIntgrt/rest/' + self.QC_domain + '/' + self.QC_project + '/run'
            post = requests.post(url=post_instance_url, headers=post_headers, data=data, verify=False)
            logging.info('Case instance id {} has been update with staus {}'.format(test_instance, status))
            if post.status_code != 200:
                logging.ERROR("Post Error for instance {}".format(test_instance))


class BasicFuncs(object):
    @staticmethod
    def default(default_value, given_value):
        if given_value == '':
            return default_value
        else:
            return given_value


# basic info
default_owner = 'Tong'
QC_domain = 'Domain'
QC_project = 'Project'
given_owner = input("Please enter the owner:\n****** Select default value 'tester', just press 'Enter' ***\n")
QC_owner = BasicFuncs.default(default_owner, given_owner)

testset_id = input("Please enter test set ID:\n")

host = 'http://HOST/'
basic_url = host + 'QCIntgr2/rest/rest.php'
suffix_url = '/domains/' + QC_domain + '/projects/' + QC_project
get_instance_url = '/test-instances?filter={cycle-id[' + str(testset_id) + ']}'
get_instance_url = basic_url + suffix_url + get_instance_url
get_filed_url = '/lists/run'
get_filed_url = basic_url + suffix_url + get_filed_url

get_headers = {
    'Accept': 'application/xml',
    'APIKey': 'Tvkw5LAMNdENQexWKpM5IA8ARZyMCquw',
}
post_headers = copy.deepcopy(get_headers)
post_headers.update({'Content-Type': 'application/x-www-form-urlencoded'})


test = HpUpdate(QC_owner, QC_domain, QC_project)
all_status = test.get_field(get_filed_url, get_headers)
i = 1
for instance_stauts in all_status:
    print("[{}] {}".format(i, instance_stauts))
    i += 1

status_id = input("Please choose the test status:\n")
status = all_status[int(status_id) - 1]

build = input("Please enter the build:\n")


# get instance info
test_instances, test_statuses, test_ids = test.get_instance(get_instance_url, get_headers)


# post instance info
test.post_method(test_instances, post_headers, status, build)


```

Also u can refer the HPQC REST API.

https://wiki.eng.vmware.com/QEOps/InfTools/HPQC-WebServices

https://wiki.eng.vmware.com/QEOps/InfTools/ALM-WebServices
