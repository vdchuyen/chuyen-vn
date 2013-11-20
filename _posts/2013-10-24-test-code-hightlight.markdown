---
layout: post
title:  "Code hightlight"
date:   2013-10-24 15:31:42
categories: blog
---

This script will scan the register and expire date or a list domains:



{% highlight python %}
#-------------------------------------------------------------------------------
# Name:        domain_expire_scanner
# Purpose:     Scan register,expire date of domains in a csv and return a csv
#
# Author:      vdchuyen@gmail.com
#
# Created:     16/09/2013
# Updated:     11/20/2013
# Copyright:   (c) chuyenvd 2013
# Licence:     @ffffff
#-------------------------------------------------------------------------------
import urllib2
import sys
import getopt
import csv
import re
import datetime
from socket import timeout


def main(argv):
    if argv is None:
        argv = sys.argv
##    Read csv and call whois
##    whois('tingme24.com')

    domains = [ x[0] for x in csv.reader(open('src-domains.csv','r')) ]

    with open('dst-domains.csv', 'wb') as domainlist:
        domain_list = csv.writer(domainlist, delimiter=',')
        for domain in domains:
            #Check if TLD domain is valid:
            domain_regex = re.compile(r'^[a-zA-Z\d-]{,63}(\.[a-zA-Z\d-]{,63})*$')
            domain_valid = re.match(domain_regex, domain)
            if (domain_valid == None):
                domain_list.writerow([domain_valid, 'domain invalid'])
            else:
                domain_list.writerow(whois(domain_valid.group()))
##                time.sleep(5)
    domainlist.close()


def whois(domain):

    result = [domain, 'Get data failed']
    try:
        whois_info = urllib2.urlopen("http://www.whois.net.vn/whois.php?domain=" + domain + "&act=getwhois ", timeout = 30)
        data_whois = whois_info.read()
        if ('chua dang ky' in data_whois):
            return result
        elif '.vn' in domain:
                register = [line for line in data_whois.split('\n') if ('Issue') in line]
                expire = [line for line in data_whois.split('\n') if ('Expir') in line]
                register_date = re.search(r'(\d+/\d+/\d+)', str(register[0:1]))
                expire_date = re.search(r'(\d+/\d+/\d+)', str(expire[0:1]))
                if register_date != None and expire_date != None:
                    re_arr = register_date.group(0)
                    ex_arr = expire_date.group(0)
                    re_arr_date = datetime.datetime.strptime(re_arr, "%d/%m/%Y").date()
                    ex_arr_date = datetime.datetime.strptime(ex_arr, "%d/%m/%Y").date()
                else:
                     return result
        else:

                register = [line for line in data_whois.split('\n') if ('Creat') in line]
                expire = [line for line in data_whois.split('\n') if ('Expir') in line]
                register_date = re.search(r'(\d{2})-(\w{3})-(\d{4})', str(register[0:1]))
                expire_date = re.search(r'(\d{2})-(\w{3})-(\d{4})', str(expire[0:1]))
                if register_date != None and expire_date != None:
                    re_arr = register_date.group(0)
                    ex_arr = expire_date.group(0)
                    re_arr_date = datetime.datetime.strptime(re_arr, "%d-%b-%Y").date()
                    ex_arr_date = datetime.datetime.strptime(ex_arr, "%d-%b-%Y").date()
                else:
                    return result
        result = [domain, re_arr_date.strftime("%Y-%m-%d"), ex_arr_date.strftime("%Y-%m-%d")]
        print result
        return result
    except urllib2.URLError as error:
        print 'Get data failed'
        return result

    except timeout:
        print 'Socket timed out'

if __name__ == "__main__":
        main(sys.argv[1:])
        
        
{% endhighlight %}
