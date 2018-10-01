# MAC Address Age Tracking

This repository is used to determine an approximate issuance date for 
IEEE allocated hardware address ranges. The dataset was bootstrapped
using a combination of the [DeepMAC](http://www.deepmac.org) and
[Wireshark](http://www.wireshark.org) archives and maintained via
daily pulls from the IEEE website.

## Usage

If you would like to use the MAC address age dataset in your application, download regular snapshots of the [mac-ages.csv](https://raw.githubusercontent.com/hdm/mac-ages/master/data/mac-ages.csv) from this repository. This file contains three comma-separated fields; the prefix followed by a forward slash and the mask, the first date this prefix was seen, and the source of this date field. This dataset is updated daily from the IEEE CSV files and new file revisions are checked into the master branch as updates are found.

If you would like to maintain a fork of this repository, you need a system with a recent version of Ruby (2.2+), and to run the `update` script in the main directory at whatever interval makes sense. This script will load the current dataset, download the IEEE CSV files, update records as necessary, save the new dataset, and commit the results back to the repository, pushing changes to the `master` branch of the remote `origin`.

## Matching

In order to match a given MAC address to the IEEE datasets, the entries with the largest mask must be checked first (/36), then the next largest (/28), and so on (/24). Matching just the first three bytes is not sufficient for accurate vendor identification, as some prefixes are split into multiple smaller ranges, each corresponding with a different vendor.

For example, when matching the MAC address `70:b3:d5:fe:90:1e`, there are 2,681 entries which match the first three bytes. These include the base allocation from 2014 (`70:b3:d5/24`) and thousands of /36 masked prefixes. Searching for the /36 masked prefixes first identifies the device with prefix `70:b3:d5:fe:90:00/36`, which corresponds to `Zenros ApS` from Denmark.

## Credit

The [DeepMAC](http://www.deepmac.org) project should be credited for coming up with the idea of MAC address age tracking. Please support the DeepMAC project if you find this capability useful. 
