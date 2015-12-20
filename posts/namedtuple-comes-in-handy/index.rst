.. title: namedtuple Comes in Handy
.. slug: namedtuple-comes-in-handy
.. date: 2015-12-20 08:58:42 UTC+08:00
.. tags: python, PyCharm, namedtuple, coding,
.. category:
.. link:
.. description: The one where I discover how useful the collections package can be.
.. type: text

I've been writing a lot of Python code recently. Oftentimes I struggle with what a method should return when I have to relay more than one value back to the caller. For example:

.. code:: python

    def PaymentGateway:
        def do_transaction(self, target, amount, bill_code, **kwargs):
            """
            Perform some transaction against the API.

            :return: whether the transaction was successful or not
            :rtype: bool
            """
            # stuff happens here
            try:
                result = self.amount_transaction(tx_details)
                logger.info("Success: CODE=%s Details=%s" % (result.code, result.detail))
                return True
            except GatewayException as ex:
                logger.error("Transaction failed: ERROR=%s reason=%s" % (ex.err_code, ex.message))
                return False

The code that calls `do_transaction`:code: might look like this:

.. code:: python

    if payment_gw.do_transaction(subid, amount, bill_code, service_id, ref_code) is True:
        # Hooray! Succe$$!
        report_success("Transaction for %s was successful. Check logs for status code." % subid)
    else:
        # Boo
        report_failure("Transaction failed. I don't know why...")

Many times this is fine, but what if the caller needs the details from the `amount_transaction`:code: result or the `GatewayException`:code:? A quick solution is to return a `dict`:code: :

.. code:: python

    def PaymentGateway:
        def do_transaction(self, target, amount, bill_code, **kwargs):
            """
            Perform some transaction against the API.

            :return: a dict that contains keys 'success', 'code', and 'detail'
            :rtype: dict
            """
            # stuff happens here
            try:
                result = self.amount_transaction(tx_details)
                logger.info("Success: CODE=%s Details=%s" % (result.code, result.detail))
                success_dict = {
                    'success': True,
                    'code': result.code,
                    'detail': result.detail,
                }
                return success_dict
            except GatewayException as ex:
                logger.error("Transaction failed: ERROR=%s reason=%s" % (ex.err_code, ex.message))
                error_dict = {
                    'success': False,
                    'code': ex.err_code,
                    'detail': ex.message,
                }
                return error_dict

It works but it's pretty ad-hoc. The structure of whatever `do_transaction`:code: returns won't be obvious unless you dig into the code. The caller will end up like:

.. code:: python

    payment_status = payment_gw.do_transaction(subid, amount, bill_code, service_id, ref_code)
    if payment_status['success'] is True:
        # Hooray! Succe$$!
        report_success("Transaction for %s was successful, status code %s" % (subid, payment_status['code']))
    else:
        # Boo
        report_failure("Transaction failed, because: %s" % payment_status['detail'])

.. _PyCharm: http://www.jetbrains.com/pycharm/

Now the caller is poluted with literal strings like `'success'`:code:, `'code'`:code: and `'status'`:code:. These can be hell to debug, specially if you happen to misspell one of them in your code. Even if you're using an awesome IDE like PyCharm_.

.. _namedtuple: https://docs.python.org/2/library/collections.html#collections.namedtuple

An altenative to defining these ad-hoc dict structures is to use namedtuple_ from the collections package.

.. code:: python

    from collections import namedtuple

    PaymentStatus = namedtuple('PaymentStatus', ['success', 'code', 'detail'])

    def PaymentGateway:
        def do_transaction(self, target, amount, bill_code, **kwargs):
            """
            Perform some transaction against the API.

            :return: whether the transaction was successful or not
            :rtype: PaymentStatus
            """
            # stuff happens here
            try:
                result = self.amount_transaction(tx_details)
                logger.info("Success: CODE=%s Details=%s" % (result.code, result.detail))
                return PaymentStatus(True, result.code, result.detail)
            except GatewayException as ex:
                logger.error("Transaction failed: ERROR=%s reason=%s" % (ex.err_code, ex.message))
                return PaymentStatus(False, ex.err_code, ex.message)

.. _`explicit is better than implicit`: http://www.thezenofpython.com/

`namedtuple`:code: forces us to be explicit about what `do_transaction`:code: returns. And `explicit is better than implicit`_. For the caller, this looks like:

.. code:: python

    payment_status = payment_gw.do_transaction(subid, amount, bill_code, service_id, ref_code)
    if payment_status.success is True:
        # Hooray! Succe$$!
        report_success("Transaction for %s was successful, status code %s" % (subid, payment_status.code))
    else:
        # Boo
        report_failure("Transaction failed, because: %s" % payment_status.detail)

This is almost as simple as our first example, and is free of string literals. And if you're using PyCharm, you can take advantage of the code completion which will know about the attributes of your new `namedtuple`:code: class:

.. image:: /images/pycharm_namedtuple.png

So if your code is littered with string literals as keys for return values from methods that return `dict`:code:, consider having them return a `namedtuple`:code: instead.





