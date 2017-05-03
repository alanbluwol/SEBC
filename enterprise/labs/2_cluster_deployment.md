# Services Enablement Boot Camp

## 2 - Cluster Deployment

```json
{
  "timestamp" : "2017-05-03T03:30:34.859Z",
  "clusters" : [ {
    "name" : "alanbluwol",
    "version" : "CDH5",
    "services" : [ {
      "name" : "hive",
      "type" : "HIVE",
      "config" : {
        "roleTypeConfigs" : [ {
          "roleType" : "HIVEMETASTORE",
          "items" : [ {
            "name" : "hive_metastore_java_heapsize",
            "value" : "640679936"
          } ]
        }, {
          "roleType" : "HIVESERVER2",
          "items" : [ {
            "name" : "hiveserver2_java_heapsize",
            "value" : "640679936"
          }, {
            "name" : "hiveserver2_spark_driver_memory",
            "value" : "966367641"
          }, {
            "name" : "hiveserver2_spark_executor_cores",
            "value" : "4"
          }, {
            "name" : "hiveserver2_spark_executor_memory",
            "value" : "3934152294"
          }, {
            "name" : "hiveserver2_spark_yarn_driver_memory_overhead",
            "value" : "102"
          }, {
            "name" : "hiveserver2_spark_yarn_executor_memory_overhead",
            "value" : "662"
          } ]
        } ],
        "items" : [ {
          "name" : "hive_metastore_database_host",
          "value" : "node1.alanbluwol"
        }, {
          "name" : "hive_metastore_database_name",
          "value" : "HMS"
        }, {
          "name" : "hive_metastore_database_password",
          "value" : "cloudera"
        }, {
          "name" : "hive_metastore_database_user",
          "value" : "cloudera"
        }, {
          "name" : "mapreduce_yarn_service",
          "value" : "yarn"
        }, {
          "name" : "zookeeper_service",
          "value" : "zookeeper"
        } ]
      },
      "roles" : [ {
        "name" : "hive-GATEWAY-433ccaa24369db14456f66a60d731080",
        "type" : "GATEWAY",
        "hostRef" : {
          "hostId" : "i-086a43bd697f8a95d"
        },
        "config" : {
          "items" : [ ]
        }
      }, {
        "name" : "hive-GATEWAY-6b35b2585b83476d64b297587a6d018f",
        "type" : "GATEWAY",
        "hostRef" : {
          "hostId" : "i-08e2e5b156afa2c87"
        },
        "config" : {
          "items" : [ ]
        }
      }, {
        "name" : "hive-GATEWAY-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "GATEWAY",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ ]
        }
      }, {
        "name" : "hive-GATEWAY-c052ee9fce13ab270db33750544c2d04",
        "type" : "GATEWAY",
        "hostRef" : {
          "hostId" : "i-0e0a065748a9cd385"
        },
        "config" : {
          "items" : [ ]
        }
      }, {
        "name" : "hive-HIVEMETASTORE-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "HIVEMETASTORE",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "9gwb5jzhimetj8l0fcr68nrts"
          } ]
        }
      }, {
        "name" : "hive-HIVESERVER2-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "HIVESERVER2",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "3tijk6nv815vw0tk9mh89z6c"
          } ]
        }
      } ],
      "displayName" : "Hive"
    }, {
      "name" : "zookeeper",
      "type" : "ZOOKEEPER",
      "config" : {
        "roleTypeConfigs" : [ {
          "roleType" : "SERVER",
          "items" : [ {
            "name" : "zookeeper_server_java_heapsize",
            "value" : "640679936"
          } ]
        } ],
        "items" : [ ]
      },
      "roles" : [ {
        "name" : "zookeeper-SERVER-433ccaa24369db14456f66a60d731080",
        "type" : "SERVER",
        "hostRef" : {
          "hostId" : "i-086a43bd697f8a95d"
        },
        "config" : {
          "items" : [ {
            "name" : "role_health_suppression_zookeeper_server_data_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_zookeeper_server_data_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_zookeeper_server_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_zookeeper_server_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "da4mwakrewxbyzt14irnu7dju"
          }, {
            "name" : "serverId",
            "value" : "2"
          } ]
        }
      }, {
        "name" : "zookeeper-SERVER-6b35b2585b83476d64b297587a6d018f",
        "type" : "SERVER",
        "hostRef" : {
          "hostId" : "i-08e2e5b156afa2c87"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "dohi3guu3d4pyldw27k23dj35"
          }, {
            "name" : "serverId",
            "value" : "1"
          } ]
        }
      }, {
        "name" : "zookeeper-SERVER-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "SERVER",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "7cjx621vpxa47ac7unih1do91"
          }, {
            "name" : "serverId",
            "value" : "3"
          } ]
        }
      } ],
      "displayName" : "ZooKeeper"
    }, {
      "name" : "hue",
      "type" : "HUE",
      "config" : {
        "roleTypeConfigs" : [ ],
        "items" : [ {
          "name" : "database_host",
          "value" : "node1.alanbluwol"
        }, {
          "name" : "database_name",
          "value" : "huey"
        }, {
          "name" : "database_password",
          "value" : "cloudera"
        }, {
          "name" : "database_type",
          "value" : "mysql"
        }, {
          "name" : "database_user",
          "value" : "cloudera"
        }, {
          "name" : "hive_service",
          "value" : "hive"
        }, {
          "name" : "hue_webhdfs",
          "value" : "hdfs-HTTPFS-8394796ca45a745a7b03cdf4a6d50530"
        }, {
          "name" : "oozie_service",
          "value" : "oozie"
        }, {
          "name" : "zookeeper_service",
          "value" : "zookeeper"
        } ]
      },
      "roles" : [ {
        "name" : "hue-HUE_SERVER-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "HUE_SERVER",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "580aor01y5nnk00cx931e7yre"
          }, {
            "name" : "secret_key",
            "value" : "BVTvZFwxOmwcava2ZLi0pCFbYGAJZO"
          } ]
        }
      } ],
      "displayName" : "Hue"
    }, {
      "name" : "oozie",
      "type" : "OOZIE",
      "config" : {
        "roleTypeConfigs" : [ {
          "roleType" : "OOZIE_SERVER",
          "items" : [ {
            "name" : "oozie_database_host",
            "value" : "node1.alanbluwol"
          }, {
            "name" : "oozie_database_name",
            "value" : "works"
          }, {
            "name" : "oozie_database_password",
            "value" : "cloudera"
          }, {
            "name" : "oozie_database_type",
            "value" : "mysql"
          }, {
            "name" : "oozie_database_user",
            "value" : "cloudera"
          }, {
            "name" : "oozie_java_heapsize",
            "value" : "640679936"
          } ]
        } ],
        "items" : [ {
          "name" : "hive_service",
          "value" : "hive"
        }, {
          "name" : "mapreduce_yarn_service",
          "value" : "yarn"
        }, {
          "name" : "zookeeper_service",
          "value" : "zookeeper"
        } ]
      },
      "roles" : [ {
        "name" : "oozie-OOZIE_SERVER-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "OOZIE_SERVER",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "2h3l66mtmq63qrjcmtb96b4cj"
          } ]
        }
      } ],
      "displayName" : "Oozie"
    }, {
      "name" : "yarn",
      "type" : "YARN",
      "config" : {
        "roleTypeConfigs" : [ {
          "roleType" : "GATEWAY",
          "items" : [ {
            "name" : "mapred_reduce_tasks",
            "value" : "6"
          }, {
            "name" : "mapred_submit_replication",
            "value" : "1"
          } ]
        }, {
          "roleType" : "JOBHISTORY",
          "items" : [ {
            "name" : "mr2_jobhistory_java_heapsize",
            "value" : "640679936"
          } ]
        }, {
          "roleType" : "NODEMANAGER",
          "items" : [ {
            "name" : "rm_cpu_shares",
            "value" : "1800"
          }, {
            "name" : "rm_io_weight",
            "value" : "900"
          }, {
            "name" : "yarn_nodemanager_heartbeat_interval_ms",
            "value" : "100"
          }, {
            "name" : "yarn_nodemanager_local_dirs",
            "value" : "/yarn/nm,/data01/yarn/nm"
          }, {
            "name" : "yarn_nodemanager_log_dirs",
            "value" : "/yarn/container-logs,/data01/yarn/container-logs"
          }, {
            "name" : "yarn_nodemanager_resource_cpu_vcores",
            "value" : "3"
          }, {
            "name" : "yarn_nodemanager_resource_memory_mb",
            "value" : "1024"
          } ]
        }, {
          "roleType" : "RESOURCEMANAGER",
          "items" : [ {
            "name" : "resource_manager_java_heapsize",
            "value" : "640679936"
          }, {
            "name" : "yarn_scheduler_maximum_allocation_mb",
            "value" : "5745"
          }, {
            "name" : "yarn_scheduler_maximum_allocation_vcores",
            "value" : "3"
          } ]
        } ],
        "items" : [ {
          "name" : "cm_yarn_container_usage_job_user",
          "value" : "hdfs"
        }, {
          "name" : "hdfs_service",
          "value" : "hdfs"
        }, {
          "name" : "rm_dirty",
          "value" : "false"
        }, {
          "name" : "rm_last_allocation_percentage",
          "value" : "90"
        }, {
          "name" : "yarn_service_cgroups",
          "value" : "false"
        }, {
          "name" : "yarn_service_lce_always",
          "value" : "false"
        }, {
          "name" : "zk_authorization_secret_key",
          "value" : "DczHbz7KhlK24cEawEAlF7ccQVZc08"
        }, {
          "name" : "zookeeper_service",
          "value" : "zookeeper"
        } ]
      },
      "roles" : [ {
        "name" : "yarn-JOBHISTORY-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "JOBHISTORY",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "6xrgtmwbew85xfuz1ljamwfok"
          } ]
        }
      }, {
        "name" : "yarn-NODEMANAGER-433ccaa24369db14456f66a60d731080",
        "type" : "NODEMANAGER",
        "hostRef" : {
          "hostId" : "i-086a43bd697f8a95d"
        },
        "config" : {
          "items" : [ {
            "name" : "role_health_suppression_node_manager_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_node_manager_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_nodemanager_local_data_directories_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_nodemanager_log_directories_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_nodemanager_recovery_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "aw1me1vr2wqpszvemgkynby5o"
          } ]
        }
      }, {
        "name" : "yarn-NODEMANAGER-6b35b2585b83476d64b297587a6d018f",
        "type" : "NODEMANAGER",
        "hostRef" : {
          "hostId" : "i-08e2e5b156afa2c87"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "er398g7vl57islezp727az99k"
          } ]
        }
      }, {
        "name" : "yarn-NODEMANAGER-c052ee9fce13ab270db33750544c2d04",
        "type" : "NODEMANAGER",
        "hostRef" : {
          "hostId" : "i-0e0a065748a9cd385"
        },
        "config" : {
          "items" : [ {
            "name" : "role_health_suppression_node_manager_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_node_manager_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_nodemanager_local_data_directories_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_nodemanager_log_directories_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_nodemanager_recovery_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "aplgk22i0webrdv5hehloacbr"
          } ]
        }
      }, {
        "name" : "yarn-RESOURCEMANAGER-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "RESOURCEMANAGER",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "rm_id",
            "value" : "48"
          }, {
            "name" : "role_jceks_password",
            "value" : "darlvtgyy2kknn8majkdgosjm"
          } ]
        }
      } ],
      "displayName" : "YARN (MR2 Included)"
    }, {
      "name" : "hdfs",
      "type" : "HDFS",
      "config" : {
        "roleTypeConfigs" : [ {
          "roleType" : "BALANCER",
          "items" : [ {
            "name" : "balancer_java_heapsize",
            "value" : "640679936"
          } ]
        }, {
          "roleType" : "DATANODE",
          "items" : [ {
            "name" : "datanode_java_heapsize",
            "value" : "1073741824"
          }, {
            "name" : "dfs_data_dir_list",
            "value" : "/dfs/dn,/data01/dfs/dn"
          }, {
            "name" : "dfs_datanode_du_reserved",
            "value" : "2146223308"
          }, {
            "name" : "dfs_datanode_failed_volumes_tolerated",
            "value" : "1"
          }, {
            "name" : "dfs_datanode_max_locked_memory",
            "value" : "4294967296"
          }, {
            "name" : "rm_cpu_shares",
            "value" : "200"
          }, {
            "name" : "rm_io_weight",
            "value" : "100"
          } ]
        }, {
          "roleType" : "GATEWAY",
          "items" : [ {
            "name" : "dfs_client_use_trash",
            "value" : "true"
          } ]
        }, {
          "roleType" : "JOURNALNODE",
          "items" : [ {
            "name" : "dfs_journalnode_edits_dir",
            "value" : "/data01/opt/cloudera/jn/"
          } ]
        }, {
          "roleType" : "NAMENODE",
          "items" : [ {
            "name" : "dfs_name_dir_list",
            "value" : "/dfs/nn,/data01/dfs/nn"
          }, {
            "name" : "dfs_namenode_servicerpc_address",
            "value" : "8022"
          } ]
        }, {
          "roleType" : "SECONDARYNAMENODE",
          "items" : [ {
            "name" : "fs_checkpoint_dir_list",
            "value" : "/dfs/snn"
          } ]
        } ],
        "items" : [ {
          "name" : "dfs_ha_fencing_cloudera_manager_secret_key",
          "value" : "2UljUv1sLjty4kpjw6FzlRXzYFwKAA"
        }, {
          "name" : "dfs_ha_fencing_methods",
          "value" : "shell(true)"
        }, {
          "name" : "fc_authorization_secret_key",
          "value" : "oxDJB00RZ7Lh6CRix0BtGvoyszHxdE"
        }, {
          "name" : "http_auth_signature_secret",
          "value" : "hG8w4Az26ztN5V27jecIMCevlP3uin"
        }, {
          "name" : "rm_dirty",
          "value" : "false"
        }, {
          "name" : "rm_last_allocation_percentage",
          "value" : "10"
        }, {
          "name" : "zookeeper_service",
          "value" : "zookeeper"
        } ]
      },
      "roles" : [ {
        "name" : "hdfs-BALANCER-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "BALANCER",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ ]
        }
      }, {
        "name" : "hdfs-DATANODE-433ccaa24369db14456f66a60d731080",
        "type" : "DATANODE",
        "hostRef" : {
          "hostId" : "i-086a43bd697f8a95d"
        },
        "config" : {
          "items" : [ {
            "name" : "role_health_suppression_data_node_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_data_node_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_datanode_data_directories_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "1a7ylh9rpayyo9n8766n7a7ry"
          } ]
        }
      }, {
        "name" : "hdfs-DATANODE-6b35b2585b83476d64b297587a6d018f",
        "type" : "DATANODE",
        "hostRef" : {
          "hostId" : "i-08e2e5b156afa2c87"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "dbqspsbkbabn6s98t8ltykvjt"
          } ]
        }
      }, {
        "name" : "hdfs-DATANODE-c052ee9fce13ab270db33750544c2d04",
        "type" : "DATANODE",
        "hostRef" : {
          "hostId" : "i-0e0a065748a9cd385"
        },
        "config" : {
          "items" : [ {
            "name" : "role_health_suppression_data_node_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_data_node_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_datanode_data_directories_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "qdtrf5d8dyrl8goeds7keabl"
          } ]
        }
      }, {
        "name" : "hdfs-FAILOVERCONTROLLER-433ccaa24369db14456f66a60d731080",
        "type" : "FAILOVERCONTROLLER",
        "hostRef" : {
          "hostId" : "i-086a43bd697f8a95d"
        },
        "config" : {
          "items" : [ {
            "name" : "role_health_suppression_hdfs_failovercontroller_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_hdfs_failovercontroller_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "327h2zad3iz8taa5cvg1ank9v"
          } ]
        }
      }, {
        "name" : "hdfs-FAILOVERCONTROLLER-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "FAILOVERCONTROLLER",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "2lrufoow2xav994uw8ju42yqx"
          } ]
        }
      }, {
        "name" : "hdfs-HTTPFS-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "HTTPFS",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "8ioj29we1ubvj1t82ztmu76qy"
          } ]
        }
      }, {
        "name" : "hdfs-JOURNALNODE-433ccaa24369db14456f66a60d731080",
        "type" : "JOURNALNODE",
        "hostRef" : {
          "hostId" : "i-086a43bd697f8a95d"
        },
        "config" : {
          "items" : [ {
            "name" : "role_health_suppression_journal_node_edits_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_journal_node_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_journal_node_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "dy0bf7uibdlfhf6izc1xtcuuk"
          } ]
        }
      }, {
        "name" : "hdfs-JOURNALNODE-6b35b2585b83476d64b297587a6d018f",
        "type" : "JOURNALNODE",
        "hostRef" : {
          "hostId" : "i-08e2e5b156afa2c87"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "bgiw2d24jlmwx1foxci32j913"
          } ]
        }
      }, {
        "name" : "hdfs-JOURNALNODE-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "JOURNALNODE",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "role_jceks_password",
            "value" : "63kl3ixsyrf9abqry3o5hy9wo"
          } ]
        }
      }, {
        "name" : "hdfs-NAMENODE-433ccaa24369db14456f66a60d731080",
        "type" : "NAMENODE",
        "hostRef" : {
          "hostId" : "i-086a43bd697f8a95d"
        },
        "config" : {
          "items" : [ {
            "name" : "autofailover_enabled",
            "value" : "true"
          }, {
            "name" : "dfs_federation_namenode_nameservice",
            "value" : "nameservice1"
          }, {
            "name" : "dfs_namenode_quorum_journal_name",
            "value" : "nameservice1"
          }, {
            "name" : "namenode_id",
            "value" : "58"
          }, {
            "name" : "role_health_suppression_name_node_data_directories_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_name_node_heap_dump_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_health_suppression_name_node_log_directory_free_space",
            "value" : "true"
          }, {
            "name" : "role_jceks_password",
            "value" : "e53tz471xgztppogudsuw0her"
          } ]
        }
      }, {
        "name" : "hdfs-NAMENODE-8394796ca45a745a7b03cdf4a6d50530",
        "type" : "NAMENODE",
        "hostRef" : {
          "hostId" : "i-0c598fea474f1c40c"
        },
        "config" : {
          "items" : [ {
            "name" : "autofailover_enabled",
            "value" : "true"
          }, {
            "name" : "dfs_federation_namenode_nameservice",
            "value" : "nameservice1"
          }, {
            "name" : "dfs_namenode_quorum_journal_name",
            "value" : "nameservice1"
          }, {
            "name" : "namenode_id",
            "value" : "50"
          }, {
            "name" : "role_jceks_password",
            "value" : "65gewsthxq2l44kuxsaw46ubx"
          } ]
        }
      } ],
      "displayName" : "HDFS"
    } ]
  } ],
  "hosts" : [ {
    "hostId" : "i-0437b65dedd0e857e",
    "ipAddress" : "172.31.1.85",
    "hostname" : "ip-172-31-1-85.ap-southeast-2.compute.internal",
    "rackId" : "/default",
    "config" : {
      "items" : [ ]
    }
  }, {
    "hostId" : "i-0c598fea474f1c40c",
    "ipAddress" : "172.31.5.116",
    "hostname" : "node1.alanbluwol",
    "rackId" : "/default",
    "config" : {
      "items" : [ {
        "name" : "host_config_suppression_memory_overcommitted_validator",
        "value" : "true"
      } ]
    }
  }, {
    "hostId" : "i-086a43bd697f8a95d",
    "ipAddress" : "172.31.7.189",
    "hostname" : "node2.alanbluwol",
    "rackId" : "/default",
    "config" : {
      "items" : [ {
        "name" : "host_config_suppression_memory_overcommitted_validator",
        "value" : "true"
      }, {
        "name" : "host_health_suppression_host_agent_parcel_directory_free_space",
        "value" : "true"
      } ]
    }
  }, {
    "hostId" : "i-08e2e5b156afa2c87",
    "ipAddress" : "172.31.13.46",
    "hostname" : "node3.alanbluwol",
    "rackId" : "/default",
    "config" : {
      "items" : [ {
        "name" : "host_config_suppression_memory_overcommitted_validator",
        "value" : "true"
      } ]
    }
  }, {
    "hostId" : "i-0e0a065748a9cd385",
    "ipAddress" : "172.31.15.253",
    "hostname" : "node4.alanbluwol",
    "rackId" : "/default",
    "config" : {
      "items" : [ {
        "name" : "host_health_suppression_host_agent_parcel_directory_free_space",
        "value" : "true"
      } ]
    }
  } ],
  "users" : [ {
    "name" : "__cloudera_internal_user__6af7841a-b69c-4f29-ade1-96cc29d49658",
    "roles" : [ "ROLE_USER" ],
    "pwHash" : "e4dcac884a743e9fb7f3f52a789af360f7f2b0a12a1cc6419df8563ab38384ed",
    "pwSalt" : -2331552940770975213,
    "pwLogin" : true
  }, {
    "name" : "__cloudera_internal_user__mgmt-EVENTSERVER-8394796ca45a745a7b03cdf4a6d50530",
    "roles" : [ "ROLE_USER" ],
    "pwHash" : "977ced5f84dff08610fddf9204c234ded7fe5af66d36d0bd47cf2f94d3c34479",
    "pwSalt" : -7169906566292251863,
    "pwLogin" : true
  }, {
    "name" : "__cloudera_internal_user__mgmt-HOSTMONITOR-8394796ca45a745a7b03cdf4a6d50530",
    "roles" : [ "ROLE_USER" ],
    "pwHash" : "6d7ff9f7b151b6660077c4f6efed6fdbdee3673263b5b19cb8f662d7969283a2",
    "pwSalt" : 7533930515514978444,
    "pwLogin" : true
  }, {
    "name" : "__cloudera_internal_user__mgmt-REPORTSMANAGER-8394796ca45a745a7b03cdf4a6d50530",
    "roles" : [ "ROLE_USER" ],
    "pwHash" : "de4684aaebaffbd33ed6ade36804d9f28acd18ffa964d6b0b4e3f5b98efb2184",
    "pwSalt" : -665305425673982885,
    "pwLogin" : true
  }, {
    "name" : "__cloudera_internal_user__mgmt-SERVICEMONITOR-8394796ca45a745a7b03cdf4a6d50530",
    "roles" : [ "ROLE_USER" ],
    "pwHash" : "f2f35bd5eac4737fc64be6bf1fbeb13f372b4d9c5642dd1ade1c259c117946bd",
    "pwSalt" : -96223705439782485,
    "pwLogin" : true
  }, {
    "name" : "admin",
    "roles" : [ "ROLE_LIMITED" ],
    "pwHash" : "fbd89246353f895892ec824d81c943c656dd781378a488a1bf28b197427e4990",
    "pwSalt" : 7394222103959143956,
    "pwLogin" : true
  }, {
    "name" : "alanbluwol",
    "roles" : [ "ROLE_ADMIN" ],
    "pwHash" : "dec0dba5041867fb1a0d936d105463a88f80b7e7a8afabcd46b3ccbc992d744e",
    "pwSalt" : -5816309630998769330,
    "pwLogin" : true
  }, {
    "name" : "minotaur",
    "roles" : [ "ROLE_CONFIGURATOR" ],
    "pwHash" : "b78e6ba2eee7838b7ad4472712c0b509e017ce159ca174c0e606edfa0eacf7c8",
    "pwSalt" : 4941767017767801541,
    "pwLogin" : true
  } ],
  "versionInfo" : {
    "version" : "5.9.2",
    "buildUser" : "jenkins",
    "buildTimestamp" : "20170330-1814",
    "gitHash" : "822da28bff818f57fc1bfc3eafe3a550300ef1b0",
    "snapshot" : false
  },
  "managementService" : {
    "name" : "mgmt",
    "type" : "MGMT",
    "config" : {
      "roleTypeConfigs" : [ {
        "roleType" : "EVENTSERVER",
        "items" : [ {
          "name" : "event_server_heapsize",
          "value" : "640679936"
        } ]
      }, {
        "roleType" : "HOSTMONITOR",
        "items" : [ {
          "name" : "firehose_non_java_memory_bytes",
          "value" : "1610612736"
        } ]
      }, {
        "roleType" : "REPORTSMANAGER",
        "items" : [ {
          "name" : "headlamp_database_host",
          "value" : "node1.alanbluwol"
        }, {
          "name" : "headlamp_database_name",
          "value" : "repman"
        }, {
          "name" : "headlamp_database_password",
          "value" : "cloudera"
        }, {
          "name" : "headlamp_database_user",
          "value" : "cloudera"
        }, {
          "name" : "headlamp_heapsize",
          "value" : "640679936"
        } ]
      }, {
        "roleType" : "SERVICEMONITOR",
        "items" : [ {
          "name" : "firehose_non_java_memory_bytes",
          "value" : "1610612736"
        } ]
      } ],
      "items" : [ ]
    },
    "roles" : [ {
      "name" : "mgmt-ALERTPUBLISHER-8394796ca45a745a7b03cdf4a6d50530",
      "type" : "ALERTPUBLISHER",
      "hostRef" : {
        "hostId" : "i-0c598fea474f1c40c"
      },
      "config" : {
        "items" : [ {
          "name" : "role_jceks_password",
          "value" : "1lmzy2tp6vczq9zn0jekylw12"
        } ]
      }
    }, {
      "name" : "mgmt-EVENTSERVER-8394796ca45a745a7b03cdf4a6d50530",
      "type" : "EVENTSERVER",
      "hostRef" : {
        "hostId" : "i-0c598fea474f1c40c"
      },
      "config" : {
        "items" : [ {
          "name" : "role_jceks_password",
          "value" : "ci260xxl9u829a0cp8ajx18bk"
        } ]
      }
    }, {
      "name" : "mgmt-HOSTMONITOR-8394796ca45a745a7b03cdf4a6d50530",
      "type" : "HOSTMONITOR",
      "hostRef" : {
        "hostId" : "i-0c598fea474f1c40c"
      },
      "config" : {
        "items" : [ {
          "name" : "role_jceks_password",
          "value" : "75u3abjqy1kwl7vksucl6guw1"
        } ]
      }
    }, {
      "name" : "mgmt-REPORTSMANAGER-8394796ca45a745a7b03cdf4a6d50530",
      "type" : "REPORTSMANAGER",
      "hostRef" : {
        "hostId" : "i-0c598fea474f1c40c"
      },
      "config" : {
        "items" : [ {
          "name" : "role_jceks_password",
          "value" : "3artkb64vky1pjj1dv6a6dav6"
        } ]
      }
    }, {
      "name" : "mgmt-SERVICEMONITOR-8394796ca45a745a7b03cdf4a6d50530",
      "type" : "SERVICEMONITOR",
      "hostRef" : {
        "hostId" : "i-0c598fea474f1c40c"
      },
      "config" : {
        "items" : [ {
          "name" : "role_jceks_password",
          "value" : "24i8ze6takoau2bmi0xzbdkgw"
        } ]
      }
    } ],
    "displayName" : "Cloudera Management Service"
  },
  "managerSettings" : {
    "items" : [ {
      "name" : "CLUSTER_STATS_START",
      "value" : "10/28/2012 1:00"
    }, {
      "name" : "PUBLIC_CLOUD_STATUS",
      "value" : "ON_PUBLIC_CLOUD"
    }, {
      "name" : "REMOTE_PARCEL_REPO_URLS",
      "value" : "https://archive.cloudera.com/cdh5/parcels/{latest_supported}/,https://archive.cloudera.com/cdh4/parcels/latest/,https://archive.cloudera.com/impala/parcels/latest/,https://archive.cloudera.com/search/parcels/latest/,https://archive.cloudera.com/accumulo/parcels/1.4/,https://archive.cloudera.com/accumulo-c5/parcels/latest/,https://archive.cloudera.com/kafka/parcels/latest/,https://archive.cloudera.com/navigator-keytrustee5/parcels/latest/,https://archive.cloudera.com/spark/parcels/latest/,https://archive.cloudera.com/sqoop-connectors/parcels/latest/"
    } ]
  }
}
```