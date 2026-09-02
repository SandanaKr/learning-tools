# Databricks CLI commands
```databricks pipelines stop b7c2ec29-7ccd-4955-86da-acc868e5245e --profile sandana-local-profile-dev```
- Stops a running pipeline in databricks environment if the profile has the access to do so

```databricks workspace export "//customer_facing/commercial/cuf_sap_foundation/state/metadata.json" --profile sandana-local-profile-dev```
- Shows the file content in the Git bash terminal from Databricks Workspace as given below
```json
{
  "version": 1,
  "config": {
    "bundle": {
      "name": "cuf_sap_foundation",
      "target": "T",
      "mode": "production",
      "git": {
        "origin_url": "https://etexit.visualstudio.com/Etex%20Portfolio%202.0/_git/da-mdp-customerfacing-databricks",
        "commit": "d51b5ed89f2d30735e95a5eef66d22f6e13a52a5",
        "bundle_root_path": "commercial/foundation"
      }
    },
    "workspace": {
      "file_path": "/Workspace/customer_facing/commercial/cuf_sap_foundation/files"
    },
    "resources": {
      "jobs": {
        "foundation": {
          "id": "30381145488683",
          "relative_path": "resources/jobs/foundation.job.yml"
        },
        "ingestion_comparison_job": {
          "id": "566490741587979",
          "relative_path": "resources/jobs/ingestion_comparison_job.yml"
        }
      },
      "pipelines": {
        "config_pipeline": {
          "id": "0617c43c-83bb-42b8-adb4-7ed1f991aac2",
          "relative_path": "resources/pipelines/config_pipeline.yml"
        },
        "foundation_pipeline_dependency": {
          "id": "5db27d7b-b58b-4008-96ac-00cf4a5b3920",
          "relative_path": "resources/pipelines/foundation_pipeline.yml"
        },
        "foundation_pipeline_s4_sales": {
          "id": "6235dd48-fc79-430a-b857-70d0ed1d1142",
          "relative_path": "resources/pipelines/foundation_pipeline.yml"
        },
        "foundation_pipeline_salesforce": {
          "id": "9d1fd37a-8027-4033-bf6e-b0ebf090a11f",
          "relative_path": "resources/pipelines/foundation_pipeline.yml"
        },
        "staging_pipeline": {
          "id": "54ec34ab-dffc-44be-ab11-475c0b176d05",
          "relative_path": "resources/pipelines/staging_pipeline.yml"
        }
      }
    },
    "presets": {
      "source_linked_deployment": false
    }
  },
  "extra": {}
}
```

```databricks bundle deploy -t T --profile sandana-local-profile-dev```
- Deploys the bundle to the target environment

```databricks bundle summary -t T --profile sandana-local-profile-dev```
This gives the below output
```
Name: cuf_sap_foundation
Target: T
Workspace:
  Host: https://adb-7181820732839861.1.azuredatabricks.net
  User: sandanakishnan.s@eteximc.com
  Path: /Workspace/customer_facing/commercial/cuf_sap_foundation
Resources:
  Jobs:
    foundation:
      Name: cuf_fdn_foundation
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/jobs/30381145488683
    ingestion_comparison_job:
      Name: cuf_fdn_ingestion_vs_foundation_comparison
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/jobs/566490741587979
  Pipelines:
    config_pipeline:
      Name: cuf_fdn_config_pipeline
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/pipelines/0617c43c-83bb-42b8-adb4-7ed1f991aac2
    foundation_pipeline_dependency:
      Name: cuf_fdn_foundation_pipeline_dependency
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/pipelines/5db27d7b-b58b-4008-96ac-00cf4a5b3920
    foundation_pipeline_marketing_performance:
      Name: cuf_fdn_foundation_pipeline_marketing_performance
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/pipelines/0e388335-1b06-4f29-bdb1-b55ad4c0ed72
    foundation_pipeline_s4_sales:
      Name: cuf_fdn_foundation_pipeline_s4_sales
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/pipelines/6235dd48-fc79-430a-b857-70d0ed1d1142
    foundation_pipeline_salesforce:
      Name: cuf_fdn_foundation_pipeline_salesforce
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/pipelines/9d1fd37a-8027-4033-bf6e-b0ebf090a11f
    staging_pipeline:
      Name: cuf_fdn_staging_pipeline
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/pipelines/54ec34ab-dffc-44be-ab11-475c0b176d05
  Schemas:
    config:
      Name: foundation_config
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/explore/data/customer_facing_commercial_d/foundation_config
  Volumes:
    canonical_config:
      Name: canonical
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/explore/data/volumes/customer_facing_commercial_d/foundation_config/canonical
    staging_config:
      Name: staging
      URL:  https://adb-7181820732839861.1.azuredatabricks.net/explore/data/volumes/customer_facing_commercial_d/foundation_config/staging
```
