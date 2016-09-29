.. title: Creating EC2 keypairs with AWS CLI
.. slug: creating-ec2-keypairs-with-aws-cli
.. date: 2016-09-29 18:48:37 UTC+08:00
.. tags: tips, AWS, JSON
.. category:
.. link:
.. description:
.. type: text

.. _`jq JSON filter`: https://stedolan.github.io/jq/

It is easy to create EC2 keypairs with the AWS CLI:

.. code-block:: shell

  $ aws ec2 create-key-pair --key-name mynewkeypair > keystuff.json

After creating the keypair it should appear in your EC2 key pairs listing. The
``keystuff.json`` file will contain the RSA private key you will need to use
to connect to any instances you create with the keypair, as well as the name of
the key and its fingerprint.

.. code-block:: text

  {
      "KeyMaterial": "-----BEGIN RSA PRIVATE KEY-----\n<your private key>==\n-----END RSA PRIVATE KEY-----",
      "KeyName": "mynewkeypair",
      "KeyFingerprint": "53:47:ee:01:3a:35:9b:52:1c:4f:99:6f:87:b0:0f:8b:ed:83:55:3b"
  }

To extract the private key into a separate file, use the
`jq JSON filter`_.

.. code-block:: shell

  $ jq '.KeyMaterial' keystuff.json --raw > mynewkey.pem
