# Execute SAP Resize Role

This Ansible role is designed to resize SAP volume and the LUNs contained within them.

## Role Workflow

### main.yml

- **Execute Task: Resize volume**
- **Execute Task `loop_luns.yml`**

### loop_luns.yml
  1. **Resize LUN**:
    - Execute `sgs.ontap.lun` role to resize the LUN

## Requirements

- NetApp ONTAP API credentials with sufficient permissions to manage volumes, and LUNs.
- Ansible collection `sgs.ontap` installed.

## Role Variables

The following variables are used by the role:

| Variable                    | Type    | Description                                  |
|-----------------------------|---------|----------------------------------------------|
| `volume`       	            | dict	  | Main object for SAP volume resizing.         |
| `volume.name`               | string	| Name of the volume to be adjusted.           |
| `volume.size_gb`            | integer	| Total size of the volume in GB.              |
| `volume.luns`               | list	  | List of LUNs contained within the volume.    |
| `volume.luns[].name`        | string	| Name of the individual LUN.                  |
| `volume.luns[].size_gb`     | integer	| Size of the LUN in GB.                       |
| `volume.luns[].mountpoint`  | string	| Mount point of the LUN in the file system.   |



#################################################################
aix_host
volume_size_gb
lun_name
lun_size_gb
mountpoint




yml format
aix_host: "server310"
volume:
  name: "gdc_fcp_sap_server310_01"
  size_gb: 64
  luns:
    - name: "server310_rootvg_10.lun"
      mountpoint: "/var/gib"
      size_gb: 41
    - name: "server310_rootvg_11.lun"
      mountpoint: "/usr/sap"
      size_gb: 22



string format
'aix_host=server310 volume={"name":"gdc_fcp_sap_server310_01","size_gb":64,"luns":[{"name":"server310_rootvg_10.lun","size_gb":41,mountpoint":"/var/gib"},{"name":"server310_rootvg_11.lun",mountpoint":"/var/gib","size_gb":22}]}'