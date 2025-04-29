# Execute SAP Clone Role

This Ansible role is designed to manage SAP volume cleanup and cloning tasks.

The `execute_sap_cloning` role is designed to automate the volume cleanup and cloning in a NetApp ONTAP environment. This role is part of the `netapp-ansible-sap-refresh` playbook and handles the core logic for identifying cleanup and clone SAP volumes.

## Role Workflow

### main.yml
N/A

### cleanup.yml
  1. **Get all LUNs from volume**:
    - Execute `sgs.ontap.rest_info` role to get all LUNs from volume

  2. **Unmap all LUNs from volumes**:
    - Execute `sgs.ontap.lun` role with task from `map.yml` to unmap all LUNs from volume

  3. **Delete volume if `delete_volume_clone_05 or delete_volume_clone_06 is true`**:
    - Execute `sgs.ontap.volume` role to delete the volume

  4. **Rename volume if `delete_volume_clone_06 is false`**:
    - Execute `sgs.ontap.volume` role to rename the volume `06_clone` to `06_clone_tobedeleted_"DateToDay + 30 days"

### clone_and_mount.yml
  1. **Get all snapshot starting with `SP_` from source volume**
    - Execute `sgs.ontap.rest_info` role to get all snapshots from the volume
  2. **Clone volume**
    - Execute `sgs.ontap.volume` role with task from `clone.yml` to clone the volume from the latest snapshot
  3.  **Get all LUNs from clone volume**:
    - Execute `sgs.ontap.rest_info` role to get all LUNs from volume
  4. **Mount all LUNs in clone volume**
    - Execute `sgs.ontap.lun` role with task from `map.yml` to mount the volumes

## Requirements

- NetApp ONTAP API credentials with sufficient permissions to manage volumes, LUNs, and initiator groups.
- Ansible collection `sgs.ontap` installed.

## Role Variables

The following variables are used by the role:

| Variable                | Type    | Description                                                   |
|-------------------------|---------|---------------------------------------------------------------|
| `volume_sap_clone`      | dict    | SAP volumes from this playbook `inventories/all.yml           |
| `vserver_sap_fc`        | string  | SAP vserver name from project inventory `inventores/sap.yml   |
| `volume_name`           | string  | Volume name for the cleanup.yml                               |
| `destination_server`    | string  | Destination server for the clone process                      |
| `delete_volume_clone_05`| bool    | handle if the clone volume _05 need to be deleted             |
| `delete_volume_clone_06`| bool    | handle if the clone volume _06 need to be deleted             |
| `skip_rename`           | bool    | handle if the volume need to be renamed                       |
