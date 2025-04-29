# SAP Delete Role

The `sap_delete` role is designed to automate the deletion of SAP-related volumes and associated resources in a NetApp ONTAP environment. This role is part of the `netapp-ansible-sap-delete-volumes` playbook and handles the core logic for identifying and removing SAP volumes, LUNs, and initiator groups.

## Role Workflow

The role performs the following tasks:

1. **Retrieve Volumes**:
   - Identifies all volumes on the specified vserver that match the AIX host name.

2. **Delete all possible snapmirrors**:
   - Identify all SnapMirror relationships and delete them without removing the destination volume.

3. **Delete Volumes**:
   - Deletes the identified volumes based on the `delete_only_sap` variable.
     - All volumes if `delete_only_sap` is `false`
     - All volumes except No. 1 and 3 if `delete_only_sap` is `true`

4. **Delete Initiator Group**:
   - Delete the associated initiator group if `delete_only_sap` is set to `false`.

## Requirements

- NetApp ONTAP API credentials with sufficient permissions to manage volumes, LUNs, and initiator groups.
- Ansible collection `sgs.ontap` installed.

## Role Variables

The following variables are used by the role:

| Variable              | Type    | Description                                                                                                 |
|-----------------------|---------|-------------------------------------------------------------------------------------------------------------|
| `delete_only_sap`     | bool    | If `true`, only SAP volumes are deleted, excluding `_01` and `_03`. If `false`, all volumes are deleted.    |
| `aix_host`            | string  | Name of the AIX host.                                                                                       |
| `vserver_sap_fc`         | string  | SAP vserver name from project inventory `inventores/sap.yml   |
