========================
 S3 compatibility tests
========================

This is a set of unofficial Amazon AWS S3 compatibility
tests, that can be useful to people implementing software
that exposes an S3-like API. The tests use the Boto2 and Boto3 libraries.

The tests use the Nose test framework. To get started, ensure you have
the ``virtualenv`` software installed; e.g. on Debian/Ubuntu::

	sudo apt-get install python-virtualenv

and then run::

	./bootstrap

You will need to create a configuration file with the location of the
service and two different credentials. A sample configuration file named
``s3tests.conf.SAMPLE`` has been provided in this repo. This file can be
used to run the s3 tests on a Ceph cluster started with vstart.

Once you have that file copied and edited, you can run the tests with::

	S3TEST_CONF=your.conf ./virtualenv/bin/nosetests

You can specify which directory of tests to run::

	S3TEST_CONF=your.conf ./virtualenv/bin/nosetests s3tests.functional

You can specify which file of tests to run::

	S3TEST_CONF=your.conf ./virtualenv/bin/nosetests s3tests.functional.test_s3

You can specify which test to run::

	S3TEST_CONF=your.conf ./virtualenv/bin/nosetests s3tests.functional.test_s3:test_bucket_list_empty

To gather a list of tests being run, use the flags::

	 -v --collect-only

Some tests have attributes set based on their current reliability and
things like AWS not enforcing their spec stricly. You can filter tests
based on their attributes::

	S3TEST_CONF=aws.conf ./virtualenv/bin/nosetests -a '!fails_on_aws'

Most of the tests have both Boto3 and Boto2 versions. Tests written in
Boto2 are in the ``s3tests`` directory. Tests written in Boto3 are
located in the ``s3test_boto3`` directory.

You can run only the boto3 tests with::

        S3TEST_CONF=your.conf ./virtualenv/bin/nosetests -v -s -A 'not fails_on_rgw' s3tests_boto3.functional


------------------------
 Added for SODA tests
------------------------
- We use this test to make sure we are aws s3 compatible, so that many commonly used s3 client can use us as s3 storage.
- As the master is moving from python2 to python3, we found some error when using the latest code of master, so we choosed a history version (c9c84faf48fd5a51009bc6d1cbd56b14af7bbc79, Fri Dec 20 18:30:21 2019) for our test.
- Currently, SODA does not support default location, that means user need to specify a backend(location) to create bucket, so we made some change to test cases accordingly.
- SODA will provide more features gradually, but currently, APIs of SODA is a subset of S3 APIs, that means not all those test cases this test program provided are supported by SODA. So we will add a new attribute, which looks like @attr('soda_test'), to those testcases we support now. You can run those testcases with this attribute as a filter. 
- **Each time when a new PR raised, those test cases with @attr('soda_test') should pass before PR is merged.** At the same time, you can run testcases without this filter to see those features we have not support. And, with more features added, more test cases will add this attribute.
