
.. _db-perf-nosql:

************************************************************************
Database Query Performance for NoSQL
************************************************************************

.. meta::
   :description: Database Query Performance can help you troubleshoot performance issues in the following NoSQL databases.

Database Query Performance can help you troubleshoot performance issues in the following NoSQL databases. For a sample use case, see :ref:`redis-use-case`.

.. _redis-db-query-performance:

Redis
================

Database Query performance shows the top commands for Redis databases called by your application. You can sort commands by total time, P90 latency, or number of requests. All versions of Redis supported by your instrumented application are compatible.

To instrument Redis for infrastructure metrics and see related Infrastructure Monitoring content, see :ref:`redis`. The following screenshot shows a sample Redis service with related infrastructure content.

.. image:: /_images/apm/db-query-perf/db-perf-redis.png
   :width: 95%
   :alt: Related content for an instrumented Redis service.

For a use case focusing on Redis and NoSQL troubleshooting, see :ref:`redis-use-case`.

.. note:: Redis databases appear as "Redis" in the :guilabel:`Services` menu.