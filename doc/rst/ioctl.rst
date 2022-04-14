.. _ioctl.h:

**ioctl.h**


Linux NVMe ioctl interface functions



.. c:struct:: nvme_passthru_cmd

   nvme passthrough command structure

**Definition**

::

  struct nvme_passthru_cmd {
    __u8 opcode;
    __u8 flags;
    __u16 rsvd1;
    __u32 nsid;
    __u32 cdw2;
    __u32 cdw3;
    __u64 metadata;
    __u64 addr;
    __u32 metadata_len;
    __u32 data_len;
    __u32 cdw10;
    __u32 cdw11;
    __u32 cdw12;
    __u32 cdw13;
    __u32 cdw14;
    __u32 cdw15;
    __u32 timeout_ms;
    __u32 result;
  };

**Members**

``opcode``
  Operation code, see :c:type:`enum nvme_io_opcodes <nvme_io_opcodes>` and :c:type:`enum nvme_admin_opcodes <nvme_admin_opcodes>`

``flags``
  Not supported: intended for command flags (eg: SGL, FUSE)

``rsvd1``
  Reserved for future use

``nsid``
  Namespace Identifier, or Fabrics type

``cdw2``
  Command Dword 2 (no spec defined use)

``cdw3``
  Command Dword 3 (no spec defined use)

``metadata``
  User space address to metadata buffer (NULL if not used)

``addr``
  User space address to data buffer (NULL if not used)

``metadata_len``
  Metadata buffer transfer length

``data_len``
  Data buffer transfer length

``cdw10``
  Command Dword 10 (command specific)

``cdw11``
  Command Dword 11 (command specific)

``cdw12``
  Command Dword 12 (command specific)

``cdw13``
  Command Dword 13 (command specific)

``cdw14``
  Command Dword 14 (command specific)

``cdw15``
  Command Dword 15 (command specific)

``timeout_ms``
  If non-zero, overrides system default timeout in milliseconds

``result``
  Set on completion to the command's CQE DWORD 0 controller response





.. c:struct:: nvme_passthru_cmd64

   64-bit nvme passthrough command structure

**Definition**

::

  struct nvme_passthru_cmd64 {
    __u8 opcode;
    __u8 flags;
    __u16 rsvd1;
    __u32 nsid;
    __u32 cdw2;
    __u32 cdw3;
    __u64 metadata;
    __u64 addr;
    __u32 metadata_len;
    __u32 data_len;
    __u32 cdw10;
    __u32 cdw11;
    __u32 cdw12;
    __u32 cdw13;
    __u32 cdw14;
    __u32 cdw15;
    __u32 timeout_ms;
    __u32 rsvd2;
    __u64 result;
  };

**Members**

``opcode``
  Operation code, see :c:type:`enum nvme_io_opcodes <nvme_io_opcodes>` and :c:type:`enum nvme_admin_opcodes <nvme_admin_opcodes>`

``flags``
  Not supported: intended for command flags (eg: SGL, FUSE)

``rsvd1``
  Reserved for future use

``nsid``
  Namespace Identifier, or Fabrics type

``cdw2``
  Command Dword 2 (no spec defined use)

``cdw3``
  Command Dword 3 (no spec defined use)

``metadata``
  User space address to metadata buffer (NULL if not used)

``addr``
  User space address to data buffer (NULL if not used)

``metadata_len``
  Metadata buffer transfer length

``data_len``
  Data buffer transfer length

``cdw10``
  Command Dword 10 (command specific)

``cdw11``
  Command Dword 11 (command specific)

``cdw12``
  Command Dword 12 (command specific)

``cdw13``
  Command Dword 13 (command specific)

``cdw14``
  Command Dword 14 (command specific)

``cdw15``
  Command Dword 15 (command specific)

``timeout_ms``
  If non-zero, overrides system default timeout in milliseconds

``rsvd2``
  Reserved for future use (and fills an impicit struct pad

``result``
  Set on completion to the command's CQE DWORD 0-1 controller response



.. c:function:: int nvme_submit_admin_passthru64 (int fd, struct nvme_passthru_cmd64 *cmd, __u64 *result)

   Submit a 64-bit nvme passthrough admin command

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_passthru_cmd64 *cmd``
  The nvme admin command to send

``__u64 *result``
  Optional field to return the result from the CQE DW0-1

**Description**

Uses NVME_IOCTL_ADMIN64_CMD for the ioctl request.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_admin_passthru64 (int fd, __u8 opcode, __u8 flags, __u16 rsvd, __u32 nsid, __u32 cdw2, __u32 cdw3, __u32 cdw10, __u32 cdw11, __u32 cdw12, __u32 cdw13, __u32 cdw14, __u32 cdw15, __u32 data_len, void *data, __u32 metadata_len, void *metadata, __u32 timeout_ms, __u64 *result)

   Submit a 64-bit nvme passthrough command

**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 opcode``
  The nvme io command to send

``__u8 flags``
  NVMe command flags (not used)

``__u16 rsvd``
  Reserevd for future use

``__u32 nsid``
  Namespace identifier

``__u32 cdw2``
  Command dword 2

``__u32 cdw3``
  Command dword 3

``__u32 cdw10``
  Command dword 10

``__u32 cdw11``
  Command dword 11

``__u32 cdw12``
  Command dword 12

``__u32 cdw13``
  Command dword 13

``__u32 cdw14``
  Command dword 14

``__u32 cdw15``
  Command dword 15

``__u32 data_len``
  Length of the data transfered in this command in bytes

``void *data``
  Pointer to user address of the data buffer

``__u32 metadata_len``
  Length of metadata transfered in this command

``void *metadata``
  Pointer to user address of the metadata buffer

``__u32 timeout_ms``
  How long the kernel waits for the command to complete

``__u64 *result``
  Optional field to return the result from the CQE dword 0

**Description**

Parameterized form of nvme_submit_admin_passthru64(). This sets up and
submits a :c:type:`struct nvme_passthru_cmd64 <nvme_passthru_cmd64>`.

Known values for **opcode** are defined in :c:type:`enum nvme_admin_opcode <nvme_admin_opcode>`.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_submit_admin_passthru (int fd, struct nvme_passthru_cmd *cmd, __u32 *result)

   Submit an nvme passthrough admin command

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_passthru_cmd *cmd``
  The nvme admin command to send

``__u32 *result``
  Optional field to return the result from the CQE DW0

**Description**

Uses NVME_IOCTL_ADMIN_CMD for the ioctl request.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_admin_passthru (int fd, __u8 opcode, __u8 flags, __u16 rsvd, __u32 nsid, __u32 cdw2, __u32 cdw3, __u32 cdw10, __u32 cdw11, __u32 cdw12, __u32 cdw13, __u32 cdw14, __u32 cdw15, __u32 data_len, void *data, __u32 metadata_len, void *metadata, __u32 timeout_ms, __u32 *result)

   Submit an nvme passthrough command

**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 opcode``
  The nvme io command to send

``__u8 flags``
  NVMe command flags (not used)

``__u16 rsvd``
  Reserevd for future use

``__u32 nsid``
  Namespace identifier

``__u32 cdw2``
  Command dword 2

``__u32 cdw3``
  Command dword 3

``__u32 cdw10``
  Command dword 10

``__u32 cdw11``
  Command dword 11

``__u32 cdw12``
  Command dword 12

``__u32 cdw13``
  Command dword 13

``__u32 cdw14``
  Command dword 14

``__u32 cdw15``
  Command dword 15

``__u32 data_len``
  Length of the data transfered in this command in bytes

``void *data``
  Pointer to user address of the data buffer

``__u32 metadata_len``
  Length of metadata transfered in this command

``void *metadata``
  Pointer to user address of the metadata buffer

``__u32 timeout_ms``
  How long the kernel waits for the command to complete

``__u32 *result``
  Optional field to return the result from the CQE dword 0

**Description**

Parameterized form of nvme_submit_admin_passthru(). This sets up and
submits a :c:type:`struct nvme_passthru_cmd <nvme_passthru_cmd>`.

Known values for **opcode** are defined in :c:type:`enum nvme_admin_opcode <nvme_admin_opcode>`.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_submit_io_passthru64 (int fd, struct nvme_passthru_cmd64 *cmd, __u64 *result)

   Submit a 64-bit nvme passthrough command

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_passthru_cmd64 *cmd``
  The nvme io command to send

``__u64 *result``
  Optional field to return the result from the CQE DW0-1

**Description**

Uses NVME_IOCTL_IO64_CMD for the ioctl request.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_io_passthru64 (int fd, __u8 opcode, __u8 flags, __u16 rsvd, __u32 nsid, __u32 cdw2, __u32 cdw3, __u32 cdw10, __u32 cdw11, __u32 cdw12, __u32 cdw13, __u32 cdw14, __u32 cdw15, __u32 data_len, void *data, __u32 metadata_len, void *metadata, __u32 timeout_ms, __u64 *result)

   Submit an nvme io passthrough command

**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 opcode``
  The nvme io command to send

``__u8 flags``
  NVMe command flags (not used)

``__u16 rsvd``
  Reserevd for future use

``__u32 nsid``
  Namespace identifier

``__u32 cdw2``
  Command dword 2

``__u32 cdw3``
  Command dword 3

``__u32 cdw10``
  Command dword 10

``__u32 cdw11``
  Command dword 11

``__u32 cdw12``
  Command dword 12

``__u32 cdw13``
  Command dword 13

``__u32 cdw14``
  Command dword 14

``__u32 cdw15``
  Command dword 15

``__u32 data_len``
  Length of the data transfered in this command in bytes

``void *data``
  Pointer to user address of the data buffer

``__u32 metadata_len``
  Length of metadata transfered in this command

``void *metadata``
  Pointer to user address of the metadata buffer

``__u32 timeout_ms``
  How long the kernel waits for the command to complete

``__u64 *result``
  Optional field to return the result from the CQE dword 0

**Description**

Parameterized form of nvme_submit_io_passthru64(). This sets up and submits
a :c:type:`struct nvme_passthru_cmd64 <nvme_passthru_cmd64>`.

Known values for **opcode** are defined in :c:type:`enum nvme_io_opcode <nvme_io_opcode>`.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_submit_io_passthru (int fd, struct nvme_passthru_cmd *cmd, __u32 *result)

   Submit an nvme passthrough command

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_passthru_cmd *cmd``
  The nvme io command to send

``__u32 *result``
  Optional field to return the result from the CQE DW0

**Description**

Uses NVME_IOCTL_IO_CMD for the ioctl request.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_io_passthru (int fd, __u8 opcode, __u8 flags, __u16 rsvd, __u32 nsid, __u32 cdw2, __u32 cdw3, __u32 cdw10, __u32 cdw11, __u32 cdw12, __u32 cdw13, __u32 cdw14, __u32 cdw15, __u32 data_len, void *data, __u32 metadata_len, void *metadata, __u32 timeout_ms, __u32 *result)

   Submit an nvme io passthrough command

**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 opcode``
  The nvme io command to send

``__u8 flags``
  NVMe command flags (not used)

``__u16 rsvd``
  Reserevd for future use

``__u32 nsid``
  Namespace identifier

``__u32 cdw2``
  Command dword 2

``__u32 cdw3``
  Command dword 3

``__u32 cdw10``
  Command dword 10

``__u32 cdw11``
  Command dword 11

``__u32 cdw12``
  Command dword 12

``__u32 cdw13``
  Command dword 13

``__u32 cdw14``
  Command dword 14

``__u32 cdw15``
  Command dword 15

``__u32 data_len``
  Length of the data transfered in this command in bytes

``void *data``
  Pointer to user address of the data buffer

``__u32 metadata_len``
  Length of metadata transfered in this command

``void *metadata``
  Pointer to user address of the metadata buffer

``__u32 timeout_ms``
  How long the kernel waits for the command to complete

``__u32 *result``
  Optional field to return the result from the CQE dword 0

**Description**

Parameterized form of nvme_submit_io_passthru(). This sets up and submits
a :c:type:`struct nvme_passthru_cmd <nvme_passthru_cmd>`.

Known values for **opcode** are defined in :c:type:`enum nvme_io_opcode <nvme_io_opcode>`.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_subsystem_reset (int fd)

   Initiate a subsystem reset

**Parameters**

``int fd``
  File descriptor of nvme device

**Description**

This should only be sent to controller handles, not to namespaces.

**Return**

Zero if a subsystem reset was initiated or -1 with errno set
otherwise.


.. c:function:: int nvme_ctrl_reset (int fd)

   Initiate a controller reset

**Parameters**

``int fd``
  File descriptor of nvme device

**Description**

This should only be sent to controller handles, not to namespaces.

**Return**

0 if a reset was initiated or -1 with errno set otherwise.


.. c:function:: int nvme_ns_rescan (int fd)

   Initiate a controller rescan

**Parameters**

``int fd``
  File descriptor of nvme device

**Description**

This should only be sent to controller handles, not to namespaces.

**Return**

0 if a rescan was initiated or -1 with errno set otherwise.


.. c:function:: int nvme_get_nsid (int fd, __u32 *nsid)

   Retrieve the NSID from a namespace file descriptor

**Parameters**

``int fd``
  File descriptor of nvme namespace

``__u32 *nsid``
  User pointer to namespace id

**Description**

This should only be sent to namespace handles, not to controllers. The
kernel's interface returns the nsid as the return value. This is unfortunate
for many architectures that are incapable of allowing distinguishing a
namespace id > 0x80000000 from a negative error number.

**Return**

0 if **nsid** was set successfully or -1 with errno set otherwise.




.. c:struct:: nvme_identify_args

   Arguments for the NVMe Identify command

**Definition**

::

  struct nvme_identify_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    enum nvme_identify_cns cns;
    enum nvme_csi csi;
    __u32 nsid;
    __u16 cntid;
    __u16 cns_specific_id;
    __u8 uuidx;
  };

**Members**

``result``
  The command completion result from CQE dword0

``data``
  User space destination address to transfer the data

``args_size``
  Size of :c:type:`struct nvme_identify_args <nvme_identify_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms (0 for default timeout)

``cns``
  The Controller or Namespace structure, see **enum** nvme_identify_cns

``csi``
  Command Set Identifier

``nsid``
  Namespace identifier, if applicable

``cntid``
  The Controller Identifier, if applicable

``cns_specific_id``
  Identifier that is required for a particular CNS value

``uuidx``
  UUID Index if controller supports this id selection method



.. c:function:: int nvme_identify (struct nvme_identify_args *args)

   Send the NVMe Identify command

**Parameters**

``struct nvme_identify_args *args``
  :c:type:`struct nvme_identify_args <nvme_identify_args>` argument structure

**Description**

The Identify command returns a data buffer that describes information about
the NVM subsystem, the controller or the namespace(s).

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_ctrl (int fd, struct nvme_id_ctrl *id)

   Retrieves nvme identify controller

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_id_ctrl *id``
  User space destination address to transfer the data,

**Description**

Sends nvme identify with CNS value ``NVME_IDENTIFY_CNS_CTRL``.

See :c:type:`struct nvme_id_ctrl <nvme_id_ctrl>` for details on the data returned.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_ns (int fd, __u32 nsid, struct nvme_id_ns *ns)

   Retrieves nvme identify namespace

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace to identify

``struct nvme_id_ns *ns``
  User space destination address to transfer the data

**Description**

If the Namespace Identifier (NSID) field specifies an active NSID, then the
Identify Namespace data structure is returned to the host for that specified
namespace.

If the controller supports the Namespace Management capability and the NSID
field is set to ``NVME_NSID_ALL``, then the controller returns an Identify Namespace
data structure that specifies capabilities that are common across namespaces
for this controller.

See :c:type:`struct nvme_id_ns <nvme_id_ns>` for details on the structure returned.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_allocated_ns (int fd, __u32 nsid, struct nvme_id_ns *ns)

   Same as nvme_identify_ns, but only for allocated namespaces

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace to identify

``struct nvme_id_ns *ns``
  User space destination address to transfer the data

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_active_ns_list (int fd, __u32 nsid, struct nvme_ns_list *list)

   Retrieves active namespaces id list

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Return namespaces greater than this identifer

``struct nvme_ns_list *list``
  User space destination address to transfer the data

**Description**

A list of 1024 namespace IDs is returned to the host containing NSIDs in
increasing order that are greater than the value specified in the Namespace
Identifier (nsid) field of the command.

See :c:type:`struct nvme_ns_list <nvme_ns_list>` for the definition of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_allocated_ns_list (int fd, __u32 nsid, struct nvme_ns_list *list)

   Retrieves allocated namespace id list

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Return namespaces greater than this identifer

``struct nvme_ns_list *list``
  User space destination address to transfer the data

**Description**

A list of 1024 namespace IDs is returned to the host containing NSIDs in
increasing order that are greater than the value specified in the Namespace
Identifier (nsid) field of the command.

See :c:type:`struct nvme_ns_list <nvme_ns_list>` for the definition of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_ctrl_list (int fd, __u16 cntid, struct nvme_ctrl_list *cntlist)

   Retrieves identify controller list

**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 cntid``
  Starting CNTLID to return in the list

``struct nvme_ctrl_list *cntlist``
  User space destination address to transfer the data

**Description**

Up to 2047 controller identifiers is returned containing a controller
identifier greater than or equal to the controller identifier  specified in
**cntid**.

See :c:type:`struct nvme_ctrl_list <nvme_ctrl_list>` for a definition of the structure returned.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_nsid_ctrl_list (int fd, __u32 nsid, __u16 cntid, struct nvme_ctrl_list *cntlist)

   Retrieves controller list attached to an nsid

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Return controllers that are attached to this nsid

``__u16 cntid``
  Starting CNTLID to return in the list

``struct nvme_ctrl_list *cntlist``
  User space destination address to transfer the data

**Description**

Up to 2047 controller identifiers are returned containing a controller
identifier greater than or equal to the controller identifier  specified in
**cntid** attached to **nsid**.

See :c:type:`struct nvme_ctrl_list <nvme_ctrl_list>` for a definition of the structure returned.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1


.. c:function:: int nvme_identify_ns_descs (int fd, __u32 nsid, struct nvme_ns_id_desc *descs)

   Retrieves namespace descriptor list

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  The namespace id to retrieve destriptors

``struct nvme_ns_id_desc *descs``
  User space destination address to transfer the data

**Description**

A list of Namespace Identification Descriptor structures is returned to the
host for the namespace specified in the Namespace Identifier (NSID) field if
it is an active NSID.

The data returned is in the form of an arrray of 'struct nvme_ns_id_desc'.

See :c:type:`struct nvme_ns_id_desc <nvme_ns_id_desc>` for the definition of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_nvmset_list (int fd, __u16 nvmsetid, struct nvme_id_nvmset_list *nvmset)

   Retrieves NVM Set List

**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 nvmsetid``
  NVM Set Identifier

``struct nvme_id_nvmset_list *nvmset``
  User space destination address to transfer the data

**Description**

Retrieves an NVM Set List, :c:type:`struct nvme_id_nvmset_list <nvme_id_nvmset_list>`. The data structure
is an ordered list by NVM Set Identifier, starting with the first NVM Set
Identifier supported by the NVM subsystem that is equal to or greater than
the NVM Set Identifier.

See :c:type:`struct nvme_id_nvmset_list <nvme_id_nvmset_list>` for the defintion of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_primary_ctrl (int fd, __u16 cntid, struct nvme_primary_ctrl_cap *cap)

   Retrieve NVMe Primary Controller identification

**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 cntid``
  Return controllers starting at this identifier

``struct nvme_primary_ctrl_cap *cap``
  User space destination buffer address to transfer the data

**Description**

See :c:type:`struct nvme_primary_ctrl_cap <nvme_primary_ctrl_cap>` for the defintion of the returned structure, **cap**.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_secondary_ctrl_list (int fd, __u32 nsid, __u16 cntid, struct nvme_secondary_ctrl_list *sc_list)

   Retrieves secondary controller list

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace identifier

``__u16 cntid``
  Return controllers starting at this identifier

``struct nvme_secondary_ctrl_list *sc_list``
  User space destination address to transfer the data

**Description**

A Secondary Controller List is returned to the host for up to 127 secondary
controllers associated with the primary controller processing this command.
The list contains entries for controller identifiers greater than or equal
to the value specified in the Controller Identifier (cntid).

See :c:type:`struct nvme_secondary_ctrls_list <nvme_secondary_ctrls_list>` for a defintion of the returned
structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_ns_granularity (int fd, struct nvme_id_ns_granularity_list *gr_list)

   Retrieves namespace granularity identification

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_id_ns_granularity_list *gr_list``
  User space destination address to transfer the data

**Description**

If the controller supports reporting of Namespace Granularity, then a
Namespace Granularity List is returned to the host for up to sixteen
namespace granularity descriptors

See :c:type:`struct nvme_id_ns_granularity_list <nvme_id_ns_granularity_list>` for the definition of the returned
structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_uuid (int fd, struct nvme_id_uuid_list *uuid_list)

   Retrieves device's UUIDs

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_id_uuid_list *uuid_list``
  User space destination address to transfer the data

**Description**

Each UUID List entry is either 0h, the NVMe Invalid UUID, or a valid UUID.
Valid UUIDs are those which are non-zero and are not the NVMe Invalid UUID.

See :c:type:`struct nvme_id_uuid_list <nvme_id_uuid_list>` for the definition of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_ns_csi (int fd, __u32 nsid, __u8 uuidx, enum nvme_csi csi, void *data)

   I/O command set specific identify namespace data

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace to identify

``__u8 uuidx``
  UUID Index for differentiating vendor specific encoding

``enum nvme_csi csi``
  Command Set Identifier

``void *data``
  User space destination address to transfer the data

**Description**

An I/O Command Set specific Identify Namespace data structre is returned
for the namespace specified in **nsid**.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_ctrl_csi (int fd, enum nvme_csi csi, void *data)

   I/O command set specific Identify Controller data

**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_csi csi``
  Command Set Identifier

``void *data``
  User space destination address to transfer the data

**Description**

An I/O Command Set specific Identify Controller data structure is returned
to the host for the controller processing the command. The specific Identify
Controller data structure to be returned is specified by **csi**.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_active_ns_list_csi (int fd, __u32 nsid, enum nvme_csi csi, struct nvme_ns_list *ns_list)

   Active namespace ID list associated with a specified I/O command set

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Return namespaces greater than this identifier

``enum nvme_csi csi``
  Command Set Identifier

``struct nvme_ns_list *ns_list``
  User space destination address to transfer the data

**Description**

A list of 1024 namespace IDs is returned to the host containing active
NSIDs in increasing order that are greater than the value specified in
the Namespace Identifier (nsid) field of the command and matching the
I/O Command Set specified in the **csi** argument.

See :c:type:`struct nvme_ns_list <nvme_ns_list>` for the definition of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_allocated_ns_list_csi (int fd, __u32 nsid, enum nvme_csi csi, struct nvme_ns_list *ns_list)

   Allocated namespace ID list associated with a specified I/O command set

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Return namespaces greater than this identifier

``enum nvme_csi csi``
  Command Set Identifier

``struct nvme_ns_list *ns_list``
  User space destination address to transfer the data

**Description**

A list of 1024 namespace IDs is returned to the host containing allocated
NSIDs in increasing order that are greater than the value specified in
the **nsid** field of the command and matching the I/O Command Set
specified in the **csi** argument.

See :c:type:`struct nvme_ns_list <nvme_ns_list>` for the definition of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_independent_identify_ns (int fd, __u32 nsid, struct nvme_id_independent_id_ns *ns)

   I/O command set independent Identify namespace data

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Return namespaces greater than this identifier

``struct nvme_id_independent_id_ns *ns``
  I/O Command Set Independent Identify Namespace data
  structure

**Description**

The I/O command set independent Identify namespace data structure for
the namespace identified with **ns** is returned to the host.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_ns_csi_user_data_format (int fd, __u16 user_data_format, __u8 uuidx, enum nvme_csi csi, void *data)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 user_data_format``
  Return namespaces capability of identifier

``__u8 uuidx``
  UUID selection, if supported

``enum nvme_csi csi``
  Command Set Identifier

``void *data``
  *undescribed*

**Description**

Identify Namespace data structure for the specified User Data Format
index containing the namespace capabilities for the NVM Command Set.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_iocs_ns_csi_user_data_format (int fd, __u16 user_data_format, __u8 uuidx, enum nvme_csi csi, void *data)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 user_data_format``
  Return namespaces capability of identifier

``__u8 uuidx``
  UUID selection, if supported

``enum nvme_csi csi``
  Command Set Identifier

``void *data``
  *undescribed*

**Description**

I/O Command Set specific Identify Namespace data structure for
the specified User Data Format index containing the namespace
capabilities for the I/O Command Set specified in the CSI field.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_nvm_identify_ctrl (int fd, struct nvme_id_ctrl_nvm *id)

   Identify controller data

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_id_ctrl_nvm *id``
  User space destination address to transfer the data

**Description**

Return an identify controller data structure to the host of
processing controller.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_domain_list (int fd, __u16 domid, struct nvme_id_domain_list *list)

   Domain list data

**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 domid``
  Domain ID

``struct nvme_id_domain_list *list``
  User space destiantion address to transfer data

**Description**

A list of 31 domain IDs is returned to the host containing domain
attributes in increasing order that are greater than the value
specified in the **domid** field.

See :c:type:`struct nvme_identify_domain_attr <nvme_identify_domain_attr>` for the definition of the
returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_endurance_group_list (int fd, __u16 endgrp_id, struct nvme_id_endurance_group_list *list)

   Endurance group list data

**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 endgrp_id``
  Endurance group identifier

``struct nvme_id_endurance_group_list *list``
  Array of endurance group identifiers

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_identify_iocs (int fd, __u16 cntlid, struct nvme_id_iocs *iocs)

   I/O command set data structure

**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 cntlid``
  Controller ID

``struct nvme_id_iocs *iocs``
  User space destination address to transfer the data

**Description**

Retrieves list of the controller's supported io command set vectors. See
:c:type:`struct nvme_id_iocs <nvme_id_iocs>`.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_zns_identify_ns (int fd, __u32 nsid, struct nvme_zns_id_ns *data)

   ZNS identify namespace data

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace to identify

``struct nvme_zns_id_ns *data``
  User space destination address to transfer the data

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_zns_identify_ctrl (int fd, struct nvme_zns_id_ctrl *id)

   ZNS identify controller data

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_zns_id_ctrl *id``
  User space destination address to transfer the data

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_get_log_args

   Arguments for the NVMe Admin Get Log command

**Definition**

::

  struct nvme_get_log_args {
    __u64 lpo;
    __u32 *result;
    void *log;
    int args_size;
    int fd;
    __u32 timeout;
    enum nvme_cmd_get_log_lid lid;
    __u32 len;
    __u32 nsid;
    enum nvme_csi csi;
    __u16 lsi;
    __u8 lsp;
    __u8 uuidx;
    bool rae;
    bool ot;
  };

**Members**

``lpo``
  Log page offset for partial log transfers

``result``
  The command completion result from CQE dword0

``log``
  User space destination address to transfer the data

``args_size``
  Length of the structure

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``lid``
  Log page identifier, see :c:type:`enum nvme_cmd_get_log_lid <nvme_cmd_get_log_lid>` for known
  values

``len``
  Length of provided user buffer to hold the log data in bytes

``nsid``
  Namespace identifier, if applicable

``csi``
  Command set identifier, see :c:type:`enum nvme_csi <nvme_csi>` for known values

``lsi``
  Log Specific Identifier

``lsp``
  Log specific field

``uuidx``
  UUID selection, if supported

``rae``
  Retain asynchronous events

``ot``
  Offset Type; if set **lpo** specifies the index into the list
  of data structures, otherwise **lpo** specifies the byte offset
  into the log page.



.. c:function:: int nvme_get_log (struct nvme_get_log_args *args)

   NVMe Admin Get Log command

**Parameters**

``struct nvme_get_log_args *args``
  :c:type:`struct nvme_get_log_args <nvme_get_log_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_supported_log_pages (int fd, bool rae, struct nvme_supported_log_pages *log)

   Retrieve nmve supported log pages

**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``struct nvme_supported_log_pages *log``
  Array of LID supported and Effects data structures

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_error (int fd, unsigned nr_entries, bool rae, struct nvme_error_log_page *err_log)

   Retrieve nvme error log

**Parameters**

``int fd``
  File descriptor of nvme device

``unsigned nr_entries``
  Number of error log entries allocated

``bool rae``
  Retain asynchronous events

``struct nvme_error_log_page *err_log``
  Array of error logs of size 'entries'

**Description**

This log page describes extended error information for a command that
completed with error, or may report an error that is not specific to a
particular command.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_smart (int fd, __u32 nsid, bool rae, struct nvme_smart_log *smart_log)

   Retrieve nvme smart log

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Optional namespace identifier

``bool rae``
  Retain asynchronous events

``struct nvme_smart_log *smart_log``
  User address to store the smart log

**Description**

This log page provides SMART and general health information. The information
provided is over the life of the controller and is retained across power
cycles. To request the controller log page, the namespace identifier
specified is FFFFFFFFh. The controller may also support requesting the log
page on a per namespace basis, as indicated by bit 0 of the LPA field in the
Identify Controller data structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_fw_slot (int fd, bool rae, struct nvme_firmware_slot *fw_log)

   Retrieves the controller firmware log

**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``struct nvme_firmware_slot *fw_log``
  User address to store the log page

**Description**

This log page describes the firmware revision stored in each firmware slot
supported. The firmware revision is indicated as an ASCII string. The log
page also indicates the active slot number.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_changed_ns_list (int fd, bool rae, struct nvme_ns_list *ns_log)

   Retrieve namespace changed list

**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``struct nvme_ns_list *ns_log``
  User address to store the log page

**Description**

This log page describes namespaces attached to this controller that have
changed since the last time the namespace was identified, been added, or
deleted.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_cmd_effects (int fd, enum nvme_csi csi, struct nvme_cmd_effects_log *effects_log)

   Retrieve nvme command effects log

**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_csi csi``
  Command Set Identifier

``struct nvme_cmd_effects_log *effects_log``
  User address to store the effects log

**Description**

This log page describes the commands that the controller supports and the
effects of those commands on the state of the NVM subsystem.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_device_self_test (int fd, struct nvme_self_test_log *log)

   Retrieve the device self test log

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_self_test_log *log``
  Userspace address of the log payload

**Description**

The log page indicates the status of an in progress self test and the
percent complete of that operation, and the results of the previous 20
self-test operations.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_create_telemetry_host (int fd, struct nvme_telemetry_log *log)

   Create host telemetry log

**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_telemetry_log *log``
  Userspace address of the log payload


.. c:function:: int nvme_get_log_telemetry_host (int fd, __u64 offset, __u32 len, void *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u64 offset``
  Offset into the telemetry data

``__u32 len``
  Length of provided user buffer to hold the log data in bytes

``void *log``
  User address for log page data

**Description**

Retreives the Telemetry Host-Initiated log page at the requested offset
using the previously existing capture.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_telemetry_ctrl (int fd, bool rae, __u64 offset, __u32 len, void *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``__u64 offset``
  Offset into the telemetry data

``__u32 len``
  Length of provided user buffer to hold the log data in bytes

``void *log``
  User address for log page data


.. c:function:: int nvme_get_log_endurance_group (int fd, __u16 endgid, struct nvme_endurance_group_log *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 endgid``
  Starting group identifier to return in the list

``struct nvme_endurance_group_log *log``
  User address to store the endurance log

**Description**

This log page indicates if an Endurance Group Event has occurred for a
particular Endurance Group. If an Endurance Group Event has occurred, the
details of the particular event are included in the Endurance Group
Information log page for that Endurance Group. An asynchronous event is
generated when an entry for an Endurance Group is newly added to this log
page.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_predictable_lat_nvmset (int fd, __u16 nvmsetid, struct nvme_nvmset_predictable_lat_log *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 nvmsetid``
  NVM set id

``struct nvme_nvmset_predictable_lat_log *log``
  User address to store the predictable latency log

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_predictable_lat_event (int fd, bool rae, __u32 offset, __u32 len, void *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``__u32 offset``
  Offset into the predictable latency event

``__u32 len``
  Length of provided user buffer to hold the log data in bytes

``void *log``
  User address for log page data


.. c:function:: int nvme_get_log_ana (int fd, enum nvme_log_ana_lsp lsp, bool rae, __u64 offset, __u32 len, void *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_log_ana_lsp lsp``
  Log specific, see :c:type:`enum nvme_get_log_ana_lsp <nvme_get_log_ana_lsp>`

``bool rae``
  Retain asynchronous events

``__u64 offset``
  Offset to the start of the log page

``__u32 len``
  The allocated length of the log page

``void *log``
  User address to store the ana log

**Description**

This log consists of a header describing the log and descriptors containing
the asymmetric namespace access information for ANA Groups that contain
namespaces that are attached to the controller processing the command.

See :c:type:`struct nvme_ana_rsp_hdr <nvme_ana_rsp_hdr>` for the defintion of the returned structure.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_ana_groups (int fd, bool rae, __u32 len, struct nvme_ana_group_desc *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``__u32 len``
  The allocated length of the log page

``struct nvme_ana_group_desc *log``
  User address to store the ana group log

**Description**

See :c:type:`struct nvme_ana_group_desc <nvme_ana_group_desc>` for the defintion of the returned structure.


.. c:function:: int nvme_get_log_lba_status (int fd, bool rae, __u64 offset, __u32 len, void *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``__u64 offset``
  Offset to the start of the log page

``__u32 len``
  The allocated length of the log page

``void *log``
  User address to store the log page


.. c:function:: int nvme_get_log_endurance_grp_evt (int fd, bool rae, __u32 offset, __u32 len, void *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``__u32 offset``
  Offset to the start of the log page

``__u32 len``
  The allocated length of the log page

``void *log``
  User address to store the log page


.. c:function:: int nvme_get_log_fid_supported_effects (int fd, bool rae, struct nvme_fid_supported_effects_log *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``struct nvme_fid_supported_effects_log *log``
  FID Supported and Effects data structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise


.. c:function:: int nvme_get_log_mi_cmd_supported_effects (int fd, bool rae, struct nvme_mi_cmd_supported_effects_log *log)

   displays the MI Commands Supported byt the controller

**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``struct nvme_mi_cmd_supported_effects_log *log``
  MI Command Supported and Effects data structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise


.. c:function:: int nvme_get_log_boot_partition (int fd, bool rae, __u8 lsp, __u32 len, struct nvme_boot_partition *part)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``__u8 lsp``
  The log specified field of LID

``__u32 len``
  The allocated size, minimum
  struct nvme_boot_partition

``struct nvme_boot_partition *part``
  User address to store the log page

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise


.. c:function:: int nvme_get_log_discovery (int fd, bool rae, __u32 offset, __u32 len, void *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``__u32 offset``
  Offset of this log to retrieve

``__u32 len``
  The allocated size for this portion of the log

``void *log``
  User address to store the discovery log

**Description**

Supported only by fabrics discovery controllers, returning discovery
records.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_media_unit_stat (int fd, __u16 domid, struct nvme_media_unit_stat_log *mus)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 domid``
  Domain Identifier selection, if supported

``struct nvme_media_unit_stat_log *mus``
  User address to store the Media Unit statistics log

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise


.. c:function:: int nvme_get_log_support_cap_config_list (int fd, __u16 domid, struct nvme_supported_cap_config_list_log *cap)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 domid``
  Domain Identifier selection, if supported

``struct nvme_supported_cap_config_list_log *cap``
  *undescribed*

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise


.. c:function:: int nvme_get_log_reservation (int fd, bool rae, struct nvme_resv_notification_log *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``struct nvme_resv_notification_log *log``
  User address to store the reservation log


.. c:function:: int nvme_get_log_sanitize (int fd, bool rae, struct nvme_sanitize_log_page *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool rae``
  Retain asynchronous events

``struct nvme_sanitize_log_page *log``
  User address to store the sanitize log

**Description**

The Sanitize Status log page reports sanitize operation time estimates and
information about the most recent sanitize operation.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_zns_changed_zones (int fd, __u32 nsid, bool rae, struct nvme_zns_changed_zone_log *log)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``bool rae``
  Retain asynchronous events

``struct nvme_zns_changed_zone_log *log``
  User address to store the changed zone log

**Description**

The list of zones that have changed state due to an exceptional event.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_log_persistent_event (int fd, enum nvme_pevent_log_action action, __u32 size, void *pevent_log)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_pevent_log_action action``
  Action the controller should take during processing this command

``__u32 size``
  Size of **pevent_log**

``void *pevent_log``
  User address to store the persistent event log




.. c:struct:: nvme_set_features_args

   Arguments for the NVMe Admin Set Feature command

**Definition**

::

  struct nvme_set_features_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 cdw11;
    __u32 cdw12;
    __u32 cdw13;
    __u32 cdw15;
    __u32 data_len;
    bool save;
    __u8 uuidx;
    __u8 fid;
  };

**Members**

``result``
  The command completion result from CQE dword0

``data``
  User address of feature data, if applicable

``args_size``
  Size of :c:type:`struct nvme_set_features_args <nvme_set_features_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID, if applicable

``cdw11``
  Value to set the feature to

``cdw12``
  Feature specific command dword12 field

``cdw15``
  Feature specific command dword15 field

``data_len``
  Length of feature data, if applicable, in bytes

``save``
  Save value across power states

``uuidx``
  UUID Index for differentiating vendor specific encoding

``fid``
  Feature identifier



.. c:function:: int nvme_set_features (struct nvme_set_features_args *args)

   Set a feature attribute

**Parameters**

``struct nvme_set_features_args *args``
  :c:type:`struct nvme_set_features_args <nvme_set_features_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_data (int fd, __u8 fid, __u32 nsid, __u32 cdw11, bool save, __u32 data_len, void *data, __u32 *result)

   Helper function for **nvme_set_features\(\)**

**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 fid``
  Feature identifier

``__u32 nsid``
  Namespace ID, if applicable

``__u32 cdw11``
  Value to set the feature to

``bool save``
  Save value across power states

``__u32 data_len``
  Length of feature data, if applicable, in bytes

``void *data``
  User address of feature data, if applicable

``__u32 *result``
  The command completion result from CQE dword0


.. c:function:: int nvme_set_features_simple (int fd, __u8 fid, __u32 nsid, __u32 cdw11, bool save, __u32 *result)

   Helper functionn for **nvme_set_features\(\)**

**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 fid``
  Feature identifier

``__u32 nsid``
  Namespace ID, if applicable

``__u32 cdw11``
  Value to set the feature to

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0


.. c:function:: int nvme_set_features_arbitration (int fd, __u8 ab, __u8 lpw, __u8 mpw, __u8 hpw, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 ab``
  Arbitration Burst

``__u8 lpw``
  Low Priority Weight

``__u8 mpw``
  Medium Priority Weight

``__u8 hpw``
  High Priority Weight

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_power_mgmt (int fd, __u8 ps, __u8 wh, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 ps``
  Power State

``__u8 wh``
  Workload Hint

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_lba_range (int fd, __u32 nsid, __u32 nr_ranges, bool save, struct nvme_lba_range_type *data, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``__u32 nr_ranges``
  Number of ranges in **data**

``bool save``
  Save value across power states

``struct nvme_lba_range_type *data``
  User address of feature data

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_temp_thresh (int fd, __u16 tmpth, __u8 tmpsel, enum nvme_feat_tmpthresh_thsel thsel, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 tmpth``
  Temperature Threshold

``__u8 tmpsel``
  Threshold Temperature Select

``enum nvme_feat_tmpthresh_thsel thsel``
  Threshold Type Select

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_err_recovery (int fd, __u32 nsid, __u16 tler, bool dulbe, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``__u16 tler``
  Time-limited error recovery value

``bool dulbe``
  Deallocated or Unwritten Logical Block Error Enable

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_volatile_wc (int fd, bool wce, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool wce``
  Write cache enable

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_irq_coalesce (int fd, __u8 thr, __u8 time, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 thr``
  Aggregation Threshold

``__u8 time``
  Aggregation Time

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_irq_config (int fd, __u16 iv, bool cd, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 iv``
  Interrupt Vector

``bool cd``
  Coalescing Disable

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_write_atomic (int fd, bool dn, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool dn``
  Disable Normal

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_async_event (int fd, __u32 events, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 events``
  Events to enable

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_auto_pst (int fd, bool apste, bool save, struct nvme_feat_auto_pst *apst, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool apste``
  Autonomous Power State Transition Enable

``bool save``
  Save value across power states

``struct nvme_feat_auto_pst *apst``
  Autonomous Power State Transition

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_timestamp (int fd, bool save, __u64 timestamp)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool save``
  Save value across power states

``__u64 timestamp``
  The current timestamp value to assign to this this feature

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_hctm (int fd, __u16 tmt2, __u16 tmt1, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 tmt2``
  Thermal Management Temperature 2

``__u16 tmt1``
  Thermal Management Temperature 1

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_nopsc (int fd, bool noppme, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool noppme``
  Non-Operational Power State Permissive Mode Enable

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_rrl (int fd, __u8 rrl, __u16 nvmsetid, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 rrl``
  Read recovery level setting

``__u16 nvmsetid``
  NVM set id

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_plm_config (int fd, bool enable, __u16 nvmsetid, bool save, struct nvme_plm_config *data, __u32*result)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool enable``
  Predictable Latency Enable

``__u16 nvmsetid``
  NVM Set Identifier

``bool save``
  Save value across power states

``struct nvme_plm_config *data``
  Pointer to structure nvme_plm_config

``__u32*result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_plm_window (int fd, enum nvme_feat_plm_window_select sel, __u16 nvmsetid, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_feat_plm_window_select sel``
  Window Select

``__u16 nvmsetid``
  NVM Set Identifier

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_lba_sts_interval (int fd, __u16 lsiri, __u16 lsipi, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 lsiri``
  LBA Status Information Report Interval

``__u16 lsipi``
  LBA Status Information Poll Interval

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_host_behavior (int fd, bool save, struct nvme_feat_host_behavior *data)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool save``
  Save value across power states

``struct nvme_feat_host_behavior *data``
  Pointer to structure nvme_feat_host_behavior

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_sanitize (int fd, bool nodrm, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool nodrm``
  No-Deallocate Response Mode

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_endurance_evt_cfg (int fd, __u16 endgid, __u8 egwarn, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u16 endgid``
  Endurance Group Identifier

``__u8 egwarn``
  Flags to enable warning, see :c:type:`enum nvme_eg_critical_warning_flags <nvme_eg_critical_warning_flags>`

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_sw_progress (int fd, __u8 pbslc, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u8 pbslc``
  Pre-boot Software Load Count

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_host_id (int fd, bool exhid, bool save, __u8 *hostid)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool exhid``
  Enable Extended Host Identifier

``bool save``
  Save value across power states

``__u8 *hostid``
  Host ID to set

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_resv_mask (int fd, __u32 mask, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 mask``
  Reservation Notification Mask Field

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_resv_persist (int fd, bool ptpl, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``bool ptpl``
  Persist Through Power Loss

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_set_features_write_protect (int fd, enum nvme_feat_nswpcfg_state state, bool save, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_feat_nswpcfg_state state``
  Write Protection State

``bool save``
  Save value across power states

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_get_features_args

   Arguments for the NVMe Admin Get Feature command

**Definition**

::

  struct nvme_get_features_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_get_features_sel sel;
    __u32 cdw11;
    __u32 data_len;
    __u8 fid;
    __u8 uuidx;
  };

**Members**

``result``
  The command completion result from CQE dword0

``data``
  User address of feature data, if applicable

``args_size``
  Size of :c:type:`struct nvme_get_features_args <nvme_get_features_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID, if applicable

``sel``
  Select which type of attribute to return,
  see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``cdw11``
  Feature specific command dword11 field

``data_len``
  Length of feature data, if applicable, in bytes

``fid``
  Feature identifier, see :c:type:`enum nvme_features_id <nvme_features_id>`

``uuidx``
  UUID Index for differentiating vendor specific encoding



.. c:function:: int nvme_get_features (struct nvme_get_features_args *args)

   Retrieve a feature attribute

**Parameters**

``struct nvme_get_features_args *args``
  :c:type:`struct nvme_get_features_args <nvme_get_features_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_data (int fd, enum nvme_features_id fid, __u32 nsid, __u32 data_len, void *data, __u32 *result)

   Helper function for **nvme_get_features\(\)**

**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_features_id fid``
  Feature identifier

``__u32 nsid``
  Namespace ID, if applicable

``__u32 data_len``
  Length of feature data, if applicable, in bytes

``void *data``
  User address of feature data, if applicable

``__u32 *result``
  The command completion result from CQE dword0


.. c:function:: int nvme_get_features_simple (int fd, enum nvme_features_id fid, __u32 nsid, __u32 *result)

   Helper function for **nvme_get_features\(\)**

**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_features_id fid``
  Feature identifier

``__u32 nsid``
  Namespace ID, if applicable

``__u32 *result``
  The command completion result from CQE dword0


.. c:function:: int nvme_get_features_arbitration (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_power_mgmt (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_lba_range (int fd, enum nvme_get_features_sel sel, struct nvme_lba_range_type *data, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``struct nvme_lba_range_type *data``
  User address of feature data, if applicable

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_temp_thresh (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_err_recovery (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_volatile_wc (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_num_queues (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_irq_coalesce (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_irq_config (int fd, enum nvme_get_features_sel sel, __u16 iv, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u16 iv``

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_write_atomic (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_async_event (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_auto_pst (int fd, enum nvme_get_features_sel sel, struct nvme_feat_auto_pst *apst, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``struct nvme_feat_auto_pst *apst``

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_host_mem_buf (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_timestamp (int fd, enum nvme_get_features_sel sel, struct nvme_timestamp *ts)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``struct nvme_timestamp *ts``
  Current timestamp

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_kato (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_hctm (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_nopsc (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_rrl (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_plm_config (int fd, enum nvme_get_features_sel sel, __u16 nvmsetid, struct nvme_plm_config *data, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u16 nvmsetid``
  NVM set id

``struct nvme_plm_config *data``

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_plm_window (int fd, enum nvme_get_features_sel sel, __u16 nvmsetid, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u16 nvmsetid``
  NVM set id

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_lba_sts_interval (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_host_behavior (int fd, enum nvme_get_features_sel sel, struct nvme_feat_host_behavior *data, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``struct nvme_feat_host_behavior *data``
  Poniter to structure nvme_feat_host_behavior

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_sanitize (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_endurance_event_cfg (int fd, enum nvme_get_features_sel sel, __u16 endgid, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u16 endgid``
  Endurance Group Identifier

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_sw_progress (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_host_id (int fd, enum nvme_get_features_sel sel, bool exhid, __u32 len, __u8 *hostid)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``bool exhid``
  Enable Extended Host Identifier

``__u32 len``
  Length of **hostid**

``__u8 *hostid``
  Buffer for returned host ID

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_resv_mask (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_resv_persist (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_write_protect (int fd, __u32 nsid, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_get_features_iocs_profile (int fd, enum nvme_get_features_sel sel, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``enum nvme_get_features_sel sel``
  Select which type of attribute to return, see :c:type:`enum nvme_get_features_sel <nvme_get_features_sel>`

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_format_nvm_args

   Arguments for the Format Nvme Namespace command

**Definition**

::

  struct nvme_format_nvm_args {
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_cmd_format_mset mset;
    enum nvme_cmd_format_pi pi;
    enum nvme_cmd_format_pil pil;
    enum nvme_cmd_format_ses ses;
    __u8 lbaf;
  };

**Members**

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_format_nvm_args <nvme_format_nvm_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Set to override default timeout to this value in milliseconds;
  useful for long running formats. 0 will use system default.

``nsid``
  Namespace ID to format

``mset``
  Metadata settings (extended or separated), true if extended

``pi``
  Protection information type

``pil``
  Protection information location (beginning or end), true if end

``ses``
  Secure erase settings

``lbaf``
  Logical block address format



.. c:function:: int nvme_format_nvm (struct nvme_format_nvm_args *args)

   Format nvme namespace(s)

**Parameters**

``struct nvme_format_nvm_args *args``
  :c:type:`struct nvme_format_nvme_args <nvme_format_nvme_args>` argument structure

**Description**

The Format NVM command low level formats the NVM media. This command is used
by the host to change the LBA data size and/or metadata size. A low level
format may destroy all data and metadata associated with all namespaces or
only the specific namespace associated with the command

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_ns_mgmt_args

   Arguments for NVMe Namespace Management command

**Definition**

::

  struct nvme_ns_mgmt_args {
    __u32 *result;
    struct nvme_id_ns *ns;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_ns_mgmt_sel sel;
    __u8 csi;
  };

**Members**

``result``
  NVMe command result

``ns``
  Namespace identication descriptors

``args_size``
  Size of :c:type:`struct nvme_ns_mgmt_args <nvme_ns_mgmt_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace identifier

``sel``
  Type of management operation to perform

``csi``
  Command Set Identifier



.. c:function:: int nvme_ns_mgmt (struct nvme_ns_mgmt_args *args)

   Issue a Namespace management command

**Parameters**

``struct nvme_ns_mgmt_args *args``
  :c:type:`struct nvme_ns_mgmt_args <nvme_ns_mgmt_args>` Argument structure


.. c:function:: int nvme_ns_mgmt_create (int fd, struct nvme_id_ns *ns, __u32 *nsid, __u32 timeout, __u8 csi)


**Parameters**

``int fd``
  File descriptor of nvme device

``struct nvme_id_ns *ns``
  Namespace identification that defines ns creation parameters

``__u32 *nsid``
  On success, set to the namespace id that was created

``__u32 timeout``
  Overide the default timeout to this value in milliseconds;
  set to 0 to use the system default.

``__u8 csi``
  Command Set Identifier

**Description**

On successful creation, the namespace exists in the subsystem, but is not
attached to any controller. Use the nvme_ns_attach_ctrls() to assign the
namespace to one or more controllers.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_ns_mgmt_delete (int fd, __u32 nsid)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace identifier to delete

**Description**

It is recommended that a namespace being deleted is not attached to any
controller. Use the nvme_ns_detach_ctrls() first if the namespace is still
attached.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_ns_attach_args

   Arguments for Nvme Namespace Management command

**Definition**

::

  struct nvme_ns_attach_args {
    __u32 *result;
    struct nvme_ctrl_list *ctrlist;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_ns_attach_sel sel;
  };

**Members**

``result``
  NVMe command result

``ctrlist``
  Controller list to modify attachment state of nsid

``args_size``
  Size of :c:type:`struct nvme_ns_attach_args <nvme_ns_attach_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID to execute attach selection

``sel``
  Attachment selection, see :c:type:`enum nvme_ns_attach_sel <nvme_ns_attach_sel>`



.. c:function:: int nvme_ns_attach (struct nvme_ns_attach_args *args)

   Attach or detach namespace to controller(s)

**Parameters**

``struct nvme_ns_attach_args *args``
  :c:type:`struct nvme_ns_attach_args <nvme_ns_attach_args>` Argument structure


.. c:function:: int nvme_ns_attach_ctrls (int fd, __u32 nsid, struct nvme_ctrl_list *ctrlist)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID to attach

``struct nvme_ctrl_list *ctrlist``
  Controller list to modify attachment state of nsid


.. c:function:: int nvme_ns_detach_ctrls (int fd, __u32 nsid, struct nvme_ctrl_list *ctrlist)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID to detach

``struct nvme_ctrl_list *ctrlist``
  Controller list to modify attachment state of nsid




.. c:struct:: nvme_fw_download_args

   Arguments for the NVMe Firmware Download command

**Definition**

::

  struct nvme_fw_download_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 offset;
    __u32 data_len;
  };

**Members**

``result``
  The command completion result from CQE dword0

``data``
  Userspace address of the firmware data

``args_size``
  Size of :c:type:`struct nvme_fw_download_args <nvme_fw_download_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``offset``
  Offset in the firmware data

``data_len``
  Length of data in this command in bytes



.. c:function:: int nvme_fw_download (struct nvme_fw_download_args *args)

   Download part or all of a firmware image to the controller

**Parameters**

``struct nvme_fw_download_args *args``
  :c:type:`struct nvme_fw_download_args <nvme_fw_download_args>` argument structure

**Description**

The Firmware Image Download command downloads all or a portion of an image
for a future update to the controller. The Firmware Image Download command
downloads a new image (in whole or in part) to the controller.

The image may be constructed of multiple pieces that are individually
downloaded with separate Firmware Image Download commands. Each Firmware
Image Download command includes a Dword Offset and Number of Dwords that
specify a dword range.

The new firmware image is not activated as part of the Firmware Image
Download command. Use the nvme_fw_commit() to activate a newly downloaded
image.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_fw_commit_args

   Arguments for the NVMe Firmware Commit command

**Definition**

::

  struct nvme_fw_commit_args {
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    enum nvme_fw_commit_ca action;
    __u8 slot;
    bool bpid;
  };

**Members**

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_fw_commit_args <nvme_fw_commit_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``action``
  Action to use for the firmware image, see :c:type:`enum nvme_fw_commit_ca <nvme_fw_commit_ca>`

``slot``
  Firmware slot to commit the downloaded image

``bpid``
  Set to true to select the boot partition id



.. c:function:: int nvme_fw_commit (struct nvme_fw_commit_args *args)

   Commit firmware using the specified action

**Parameters**

``struct nvme_fw_commit_args *args``
  :c:type:`struct nvme_fw_commit_args <nvme_fw_commit_args>` argument structure

**Description**

The Firmware Commit command modifies the firmware image or Boot Partitions.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise. The command
status response may specify additional reset actions required to complete
the commit process.




.. c:struct:: nvme_security_send_args

   Arguments for the NVMe Security Send command

**Definition**

::

  struct nvme_security_send_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 tl;
    __u32 data_len;
    __u8 nssf;
    __u8 spsp0;
    __u8 spsp1;
    __u8 secp;
  };

**Members**

``result``
  The command completion result from CQE dword0

``data``
  Security data payload to send

``args_size``
  Size of :c:type:`struct nvme_security_send_args <nvme_security_send_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID to issue security command on

``tl``
  Protocol specific transfer length

``data_len``
  Data length of the payload in bytes

``nssf``
  NVMe Security Specific field

``spsp0``
  Security Protocol Specific field

``spsp1``
  Security Protocol Specific field

``secp``
  Security Protocol



.. c:function:: int nvme_security_send (struct nvme_security_send_args *args)


**Parameters**

``struct nvme_security_send_args *args``
  :c:type:`struct nvme_security_send <nvme_security_send>` argument structure

**Description**

The Security Send command transfers security protocol data to the
controller. The data structure transferred to the controller as part of this
command contains security protocol specific commands to be performed by the
controller. The data structure transferred may also contain data or
parameters associated with the security protocol commands.

The security data is protocol specific and is not defined by the NVMe
specification.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_security_receive_args

   Arguments for the NVMe Security Receive command

**Definition**

::

  struct nvme_security_receive_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 al;
    __u32 data_len;
    __u8 nssf;
    __u8 spsp0;
    __u8 spsp1;
    __u8 secp;
  };

**Members**

``result``
  The command completion result from CQE dword0

``data``
  Security data payload to send

``args_size``
  Size of :c:type:`struct nvme_security_receive_args <nvme_security_receive_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID to issue security command on

``al``
  Protocol specific allocation length

``data_len``
  Data length of the payload in bytes

``nssf``
  NVMe Security Specific field

``spsp0``
  Security Protocol Specific field

``spsp1``
  Security Protocol Specific field

``secp``
  Security Protocol



.. c:function:: int nvme_security_receive (struct nvme_security_receive_args *args)


**Parameters**

``struct nvme_security_receive_args *args``
  :c:type:`struct nvme_security_recevice <nvme_security_recevice>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_get_lba_status_args

   Arguments for the NVMe Get LBA Status command

**Definition**

::

  struct nvme_get_lba_status_args {
    __u64 slba;
    __u32 *result;
    struct nvme_lba_status *lbas;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 mndw;
    enum nvme_lba_status_atype atype;
    __u16 rl;
  };

**Members**

``slba``
  Starting logical block address to check statuses

``result``
  The command completion result from CQE dword0

``lbas``
  Data payload to return status descriptors

``args_size``
  Size of :c:type:`struct nvme_get_lba_status_args <nvme_get_lba_status_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID to retrieve LBA status

``mndw``
  Maximum number of dwords to return

``atype``
  Action type mechanism to determine LBA status desctriptors to
  return, see :c:type:`enum nvme_lba_status_atype <nvme_lba_status_atype>`

``rl``
  Range length from slba to perform the action



.. c:function:: int nvme_get_lba_status (struct nvme_get_lba_status_args *args)

   Retrieve information on possibly unrecoverable LBAs

**Parameters**

``struct nvme_get_lba_status_args *args``
  :c:type:`struct nvme_get_lba_status_args <nvme_get_lba_status_args>` argument structure

**Description**

The Get LBA Status command requests information about Potentially
Unrecoverable LBAs. Refer to the specification for action type descriptions.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_directive_send_args

   Arguments for the NVMe Directive Send command

**Definition**

::

  struct nvme_directive_send_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_directive_send_doper doper;
    enum nvme_directive_dtype dtype;
    __u32 cdw12;
    __u32 data_len;
    __u16 dspec;
  };

**Members**

``result``
  If successful, the CQE dword0 value

``data``
  Data payload to to be send

``args_size``
  Size of :c:type:`struct nvme_directive_send_args <nvme_directive_send_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID, if applicable

``doper``
  Directive send operation, see :c:type:`enum nvme_directive_send_doper <nvme_directive_send_doper>`

``dtype``
  Directive type, see :c:type:`enum nvme_directive_dtype <nvme_directive_dtype>`

``cdw12``
  Directive specific command dword12

``data_len``
  Length of data payload in bytes

``dspec``
  Directive specific field



.. c:function:: int nvme_directive_send (struct nvme_directive_send_args *args)

   Send directive command

**Parameters**

``struct nvme_directive_send_args *args``
  :c:type:`struct nvme_directive_send_args <nvme_directive_send_args>` argument structure

**Description**

Directives is a mechanism to enable host and NVM subsystem or controller
information exchange. The Directive Send command transfers data related to a
specific Directive Type from the host to the controller.

See the NVMe specification for more information.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_directive_send_id_endir (int fd, __u32 nsid, bool endir, enum nvme_directive_dtype dtype, struct nvme_id_directives *id)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace Identifier

``bool endir``
  Enable Directive

``enum nvme_directive_dtype dtype``
  Directive Type

``struct nvme_id_directives *id``
  Pointer to structure nvme_id_directives

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_directive_send_stream_release_identifier (int fd, __u32 nsid, __u16 stream_id)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``__u16 stream_id``
  Stream identifier

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_directive_send_stream_release_resource (int fd, __u32 nsid)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_directive_recv_args

   Arguments for the NVMe Directive Receive command

**Definition**

::

  struct nvme_directive_recv_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_directive_receive_doper doper;
    enum nvme_directive_dtype dtype;
    __u32 cdw12;
    __u32 data_len;
    __u16 dspec;
  };

**Members**

``result``
  If successful, the CQE dword0 value

``data``
  Usespace address of data payload

``args_size``
  Size of :c:type:`struct nvme_directive_recv_args <nvme_directive_recv_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID, if applicable

``doper``
  Directive send operation, see :c:type:`enum nvme_directive_send_doper <nvme_directive_send_doper>`

``dtype``
  Directive type, see :c:type:`enum nvme_directive_dtype <nvme_directive_dtype>`

``cdw12``
  Directive specific command dword12

``data_len``
  Length of data payload in bytes

``dspec``
  Directive specific field



.. c:function:: int nvme_directive_recv (struct nvme_directive_recv_args *args)

   Receive directive specific data

**Parameters**

``struct nvme_directive_recv_args *args``
  :c:type:`struct nvme_directive_recv_args <nvme_directive_recv_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_directive_recv_identify_parameters (int fd, __u32 nsid, struct nvme_id_directives *id)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``struct nvme_id_directives *id``
  Identify parameters buffer

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_directive_recv_stream_parameters (int fd, __u32 nsid, struct nvme_streams_directive_params *parms)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``struct nvme_streams_directive_params *parms``
  Streams directive parameters buffer

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_directive_recv_stream_status (int fd, __u32 nsid, unsigned nr_entries, struct nvme_streams_directive_status *id)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``unsigned nr_entries``
  Number of streams to receive

``struct nvme_streams_directive_status *id``
  Stream status buffer

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_directive_recv_stream_allocate (int fd, __u32 nsid, __u16 nsr, __u32 *result)


**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``__u16 nsr``
  Namespace Streams Requested

``__u32 *result``
  If successful, the CQE dword0 value

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_capacity_mgmt_args

   Arguments for the NVMe Capacity Management command

**Definition**

::

  struct nvme_capacity_mgmt_args {
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 cdw11;
    __u32 cdw12;
    __u16 element_id;
    __u8 op;
  };

**Members**

``result``
  If successful, the CQE dword0 value

``args_size``
  Size of :c:type:`struct nvme_capacity_mgmt_args <nvme_capacity_mgmt_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``cdw11``
  Least significant 32 bits of the capacity in bytes of the
  Endurance Group or NVM Set to be created

``cdw12``
  Most significant 32 bits of the capacity in bytes of the
  Endurance Group or NVM Set to be created

``element_id``
  Value specific to the value of the Operation field

``op``
  Operation to be performed by the controller



.. c:function:: int nvme_capacity_mgmt (struct nvme_capacity_mgmt_args *args)


**Parameters**

``struct nvme_capacity_mgmt_args *args``
  :c:type:`struct nvme_capacity_mgmt_args <nvme_capacity_mgmt_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_lockdown_args

   Arguments for the NVME Lockdown command

**Definition**

::

  struct nvme_lockdown_args {
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    __u8 scp;
    __u8 prhbt;
    __u8 ifc;
    __u8 ofi;
    __u8 uuidx;
  };

**Members**

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_lockdown_args <nvme_lockdown_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms (0 for default timeout)

``scp``
  Scope of the command

``prhbt``
  Prohibit or allow the command opcode or Set Features command

``ifc``
  Affected interface

``ofi``
  Opcode or Feature Identifier

``uuidx``
  UUID Index if controller supports this id selection method



.. c:function:: int nvme_lockdown (struct nvme_lockdown_args *args)

   Issue lockdown command

**Parameters**

``struct nvme_lockdown_args *args``
  :c:type:`struct nvme_lockdown_args <nvme_lockdown_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_set_property_args

   Arguments for NVMe Set Property command

**Definition**

::

  struct nvme_set_property_args {
    __u64 value;
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    int offset;
  };

**Members**

``value``
  The value to set the property

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_set_property_args <nvme_set_property_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``offset``
  Property offset from the base to set



.. c:function:: int nvme_set_property (struct nvme_set_property_args *args)

   Set controller property

**Parameters**

``struct nvme_set_property_args *args``
  :c:type:`struct nvme_set_property_args <nvme_set_property_args>` argument structure

**Description**

This is an NVMe-over-Fabrics specific command, not applicable to PCIe. These
properties align to the PCI MMIO controller registers.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_get_property_args

   Arguments for NVMe Get Property command

**Definition**

::

  struct nvme_get_property_args {
    __u64 *value;
    int args_size;
    int fd;
    __u32 timeout;
    int offset;
  };

**Members**

``value``
  Where the property's value will be stored on success

``args_size``
  Size of :c:type:`struct nvme_get_property_args <nvme_get_property_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``offset``
  Property offset from the base to retrieve



.. c:function:: int nvme_get_property (struct nvme_get_property_args *args)

   Get a controller property

**Parameters**

``struct nvme_get_property_args *args``
  :c:type:`struct nvme_get_propert_args <nvme_get_propert_args>` argument structure

**Description**

This is an NVMe-over-Fabrics specific command, not applicable to PCIe. These
properties align to the PCI MMIO controller registers.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_sanitize_nvm_args

   Arguments for the NVMe Sanitize NVM command

**Definition**

::

  struct nvme_sanitize_nvm_args {
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    enum nvme_sanitize_sanact sanact;
    __u32 ovrpat;
    bool ause;
    __u8 owpass;
    bool oipbp;
    bool nodas;
  };

**Members**

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_sanitize_nvm_args <nvme_sanitize_nvm_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``sanact``
  Sanitize action, see :c:type:`enum nvme_sanitize_sanact <nvme_sanitize_sanact>`

``ovrpat``
  Overwrite pattern

``ause``
  Set to allow unrestriced sanitize exit

``owpass``
  Overwrite pass count

``oipbp``
  Set to overwrite invert pattern between passes

``nodas``
  Set to not deallocate blocks after sanitizing



.. c:function:: int nvme_sanitize_nvm (struct nvme_sanitize_nvm_args *args)

   Start a sanitize operation

**Parameters**

``struct nvme_sanitize_nvm_args *args``
  :c:type:`struct nvme_sanitize_nvm_args <nvme_sanitize_nvm_args>` argument structure

**Description**

A sanitize operation alters all user data in the NVM subsystem such that
recovery of any previous user data from any cache, the non-volatile media,
or any Controller Memory Buffer is not possible.

The Sanitize command starts a sanitize operation or to recover from a
previously failed sanitize operation. The sanitize operation types that may
be supported are Block Erase, Crypto Erase, and Overwrite. All sanitize
operations are processed in the background, i.e., completion of the sanitize
command does not indicate completion of the sanitize operation.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_dev_self_test_args

   Arguments for the NVMe Device Self Test command

**Definition**

::

  struct nvme_dev_self_test_args {
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_dst_stc stc;
  };

**Members**

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_dev_self_test_args <nvme_dev_self_test_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID to test

``stc``
  Self test code, see :c:type:`enum nvme_dst_stc <nvme_dst_stc>`



.. c:function:: int nvme_dev_self_test (struct nvme_dev_self_test_args *args)

   Start or abort a self test

**Parameters**

``struct nvme_dev_self_test_args *args``
  :c:type:`struct nvme_dev_self_test <nvme_dev_self_test>` argument structure

**Description**

The Device Self-test command starts a device self-test operation or abort a
device self-test operation. A device self-test operation is a diagnostic
testing sequence that tests the integrity and functionality of the
controller and may include testing of the media associated with namespaces.
The controller may return a response to this command immediately while
running the self-test in the background.

Set the 'nsid' field to 0 to not include namepsaces in the test. Set to
0xffffffff to test all namespaces. All other values tests a specific
namespace, if present.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_virtual_mgmt_args

   Arguments for the NVMe Virtualization resource management command

**Definition**

::

  struct nvme_virtual_mgmt_args {
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    enum nvme_virt_mgmt_act act;
    enum nvme_virt_mgmt_rt rt;
    __u16 cntlid;
    __u16 nr;
  };

**Members**

``result``
  If successful, the CQE dword0

``args_size``
  Size of :c:type:`struct nvme_virtual_mgmt_args <nvme_virtual_mgmt_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``act``
  Virtual resource action, see :c:type:`enum nvme_virt_mgmt_act <nvme_virt_mgmt_act>`

``rt``
  Resource type to modify, see :c:type:`enum nvme_virt_mgmt_rt <nvme_virt_mgmt_rt>`

``cntlid``
  Controller id for which resources are bing modified

``nr``
  Number of resources being allocated or assigned



.. c:function:: int nvme_virtual_mgmt (struct nvme_virtual_mgmt_args *args)

   Virtualization resource management

**Parameters**

``struct nvme_virtual_mgmt_args *args``
  :c:type:`struct nvme_virtual_mgmt_args <nvme_virtual_mgmt_args>` argument structure

**Description**

The Virtualization Management command is supported by primary controllers
that support the Virtualization Enhancements capability. This command is
used for several functions:

     - Modifying Flexible Resource allocation for the primary controller
     - Assigning Flexible Resources for secondary controllers
     - Setting the Online and Offline state for secondary controllers

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_flush (int fd, __u32 nsid)

   Send an nvme flush command

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace identifier

**Description**

The Flush command requests that the contents of volatile write cache be made
non-volatile.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_io_args

   Arguments for NVMe I/O commands

**Definition**

::

  struct nvme_io_args {
    __u64 slba;
    __u64 storage_tag;
    __u32 *result;
    void *data;
    void *metadata;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 reftag;
    __u32 data_len;
    __u32 metadata_len;
    __u16 nlb;
    __u16 control;
    __u16 apptag;
    __u16 appmask;
    __u16 dspec;
    __u8 dsm;
  };

**Members**

``slba``
  Starting logical block

``storage_tag``
  This filed specifies Variable Sized Expected Logical Block
  Storage Tag (ELBST) and Expected Logical Block Reference
  Tag (ELBRT)

``result``
  The command completion result from CQE dword0

``data``
  Pointer to user address of the data buffer

``metadata``
  Pointer to user address of the metadata buffer

``args_size``
  Size of :c:type:`struct nvme_io_args <nvme_io_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID

``reftag``
  This field specifies the Initial Logical Block Reference Tag
  expected value. Used only if the namespace is formatted to use
  end-to-end protection information.

``data_len``
  Length of user buffer, **data**, in bytes

``metadata_len``
  Length of user buffer, **metadata**, in bytes

``nlb``
  Number of logical blocks to send (0's based value)

``control``
  Command control flags, see :c:type:`enum nvme_io_control_flags <nvme_io_control_flags>`.

``apptag``
  This field specifies the Application Tag Mask expected value.
  Used only if the namespace is formatted to use end-to-end
  protection information.

``appmask``
  This field specifies the Application Tag expected value. Used
  only if the namespace is formatted to use end-to-end protection
  information.

``dspec``
  Directive specific value

``dsm``
  Data set management attributes, see :c:type:`enum nvme_io_dsm_flags <nvme_io_dsm_flags>`



.. c:function:: int nvme_io (struct nvme_io_args *args, __u8 opcode)

   Submit an nvme user I/O command

**Parameters**

``struct nvme_io_args *args``
  :c:type:`struct nvme_io_args <nvme_io_args>` argument structure

``__u8 opcode``
  Opcode to execute

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_read (struct nvme_io_args *args)

   Submit an nvme user read command

**Parameters**

``struct nvme_io_args *args``
  :c:type:`struct nvme_io_args <nvme_io_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_write (struct nvme_io_args *args)

   Submit an nvme user write command

**Parameters**

``struct nvme_io_args *args``
  :c:type:`struct nvme_io_args <nvme_io_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_compare (struct nvme_io_args *args)

   Submit an nvme user compare command

**Parameters**

``struct nvme_io_args *args``
  :c:type:`struct nvme_io_args <nvme_io_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_write_zeros (struct nvme_io_args *args)

   Submit an nvme write zeroes command

**Parameters**

``struct nvme_io_args *args``
  :c:type:`struct nvme_io_args <nvme_io_args>` argument structure

**Description**

The Write Zeroes command sets a range of logical blocks to zero.  After
successful completion of this command, the value returned by subsequent
reads of logical blocks in this range shall be all bytes cleared to 0h until
a write occurs to this LBA range.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_write_uncorrectable (struct nvme_io_args *args)

   Submit an nvme write uncorrectable command

**Parameters**

``struct nvme_io_args *args``
  :c:type:`struct nvme_io_args <nvme_io_args>` argument structure

**Description**

The Write Uncorrectable command marks a range of logical blocks as invalid.
When the specified logical block(s) are read after this operation, a failure
is returned with Unrecovered Read Error status. To clear the invalid logical
block status, a write operation on those logical blocks is required.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_verify (struct nvme_io_args *args)

   Send an nvme verify command

**Parameters**

``struct nvme_io_args *args``
  :c:type:`struct nvme_io_args <nvme_io_args>` argument structure

**Description**

The Verify command verifies integrity of stored information by reading data
and metadata, if applicable, for the LBAs indicated without transferring any
data or metadata to the host.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_dsm_args

   Arguments for the NVMe Dataset Management command

**Definition**

::

  struct nvme_dsm_args {
    __u32 *result;
    struct nvme_dsm_range *dsm;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 attrs;
    __u16 nr_ranges;
  };

**Members**

``result``
  The command completion result from CQE dword0

``dsm``
  The data set management attributes

``args_size``
  Size of :c:type:`struct nvme_dsm_args <nvme_dsm_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace identifier

``attrs``
  DSM attributes, see :c:type:`enum nvme_dsm_attributes <nvme_dsm_attributes>`

``nr_ranges``
  Number of block ranges in the data set management attributes



.. c:function:: int nvme_dsm (struct nvme_dsm_args *args)

   Send an nvme data set management command

**Parameters**

``struct nvme_dsm_args *args``
  :c:type:`struct nvme_dsm_args <nvme_dsm_args>` argument structure

**Description**

The Dataset Management command is used by the host to indicate attributes
for ranges of logical blocks. This includes attributes like frequency that
data is read or written, access size, and other information that may be used
to optimize performance and reliability, and may be used to
deallocate/unmap/trim those logical blocks.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_copy_args

   Arguments for the NVMe Copy command

**Definition**

::

  struct nvme_copy_args {
    __u64 sdlba;
    __u32 *result;
    struct nvme_copy_range *copy;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 ilbrt;
    int lr;
    int fua;
    __u16 nr;
    __u16 dspec;
    __u16 lbatm;
    __u16 lbat;
    __u8 prinfor;
    __u8 prinfow;
    __u8 dtype;
    __u8 format;
  };

**Members**

``sdlba``
  Start destination LBA

``result``
  The command completion result from CQE dword0

``copy``
  Range descriptior

``args_size``
  Size of :c:type:`struct nvme_copy_args <nvme_copy_args>`

``fd``
  File descriptor of the nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace identifier

``ilbrt``
  Initial logical block reference tag

``lr``
  Limited retry

``fua``
  Force unit access

``nr``
  Number of ranges

``dspec``
  Directive specific value

``lbatm``
  Logical block application tag mask

``lbat``
  Logical block application tag

``prinfor``
  Protection information field for read

``prinfow``
  Protection information field for write

``dtype``
  Directive type

``format``
  Descriptor format



.. c:function:: int nvme_copy (struct nvme_copy_args *args)


**Parameters**

``struct nvme_copy_args *args``
  :c:type:`struct nvme_copy_args <nvme_copy_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_resv_acquire_args

   Arguments for the NVMe Reservation Acquire Comand

**Definition**

::

  struct nvme_resv_acquire_args {
    __u64 crkey;
    __u64 nrkey;
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_resv_rtype rtype;
    enum nvme_resv_racqa racqa;
    bool iekey;
  };

**Members**

``crkey``
  The current reservation key associated with the host

``nrkey``
  The reservation key to be unregistered from the namespace if
  the action is preempt

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_resv_acquire_args <nvme_resv_acquire_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace identifier

``rtype``
  The type of reservation to be create, see :c:type:`enum nvme_resv_rtype <nvme_resv_rtype>`

``racqa``
  The action that is performed by the command, see :c:type:`enum nvme_resv_racqa <nvme_resv_racqa>`

``iekey``
  Set to ignore the existing key



.. c:function:: int nvme_resv_acquire (struct nvme_resv_acquire_args *args)

   Send an nvme reservation acquire

**Parameters**

``struct nvme_resv_acquire_args *args``
  :c:type:`struct nvme_resv_acquire <nvme_resv_acquire>` argument structure

**Description**

The Reservation Acquire command acquires a reservation on a namespace,
preempt a reservation held on a namespace, and abort a reservation held on a
namespace.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_resv_register_args

   Arguments for the NVMe Reservation Register command

**Definition**

::

  struct nvme_resv_register_args {
    __u64 crkey;
    __u64 nrkey;
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_resv_rrega rrega;
    enum nvme_resv_cptpl cptpl;
    bool iekey;
  };

**Members**

``crkey``
  The current reservation key associated with the host

``nrkey``
  The new reservation key to be register if action is register or
  replace

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_resv_register_args <nvme_resv_register_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace identifier

``rrega``
  The registration action, see :c:type:`enum nvme_resv_rrega <nvme_resv_rrega>`

``cptpl``
  Change persist through power loss, see :c:type:`enum nvme_resv_cptpl <nvme_resv_cptpl>`

``iekey``
  Set to ignore the existing key



.. c:function:: int nvme_resv_register (struct nvme_resv_register_args *args)

   Send an nvme reservation register

**Parameters**

``struct nvme_resv_register_args *args``
  :c:type:`struct nvme_resv_register_args <nvme_resv_register_args>` argument structure

**Description**

The Reservation Register command registers, unregisters, or replaces a
reservation key.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_resv_release_args

   Arguments for the NVMe Reservation Release Command

**Definition**

::

  struct nvme_resv_release_args {
    __u64 crkey;
    __u32 *result;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_resv_rtype rtype;
    enum nvme_resv_rrela rrela;
    bool iekey;
  };

**Members**

``crkey``
  The current reservation key to release

``result``
  The command completion result from CQE dword0

``args_size``
  Size of :c:type:`struct nvme_resv_release_args <nvme_resv_release_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace identifier

``rtype``
  The type of reservation to be create, see :c:type:`enum nvme_resv_rtype <nvme_resv_rtype>`

``rrela``
  Reservation releast action, see :c:type:`enum nvme_resv_rrela <nvme_resv_rrela>`

``iekey``
  Set to ignore the existing key



.. c:function:: int nvme_resv_release (struct nvme_resv_release_args *args)

   Send an nvme reservation release

**Parameters**

``struct nvme_resv_release_args *args``
  :c:type:`struct nvme_resv_release_args <nvme_resv_release_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_resv_report_args

   Arguments for the NVMe Reservation Report command

**Definition**

::

  struct nvme_resv_report_args {
    __u32 *result;
    struct nvme_resv_status *report;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 len;
    bool eds;
  };

**Members**

``result``
  The command completion result from CQE dword0

``report``
  The user space destination address to store the reservation
  report

``args_size``
  Size of :c:type:`struct nvme_resv_report_args <nvme_resv_report_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace identifier

``len``
  Number of bytes to request transfered with this command

``eds``
  Request extended Data Structure



.. c:function:: int nvme_resv_report (struct nvme_resv_report_args *args)

   Send an nvme reservation report

**Parameters**

``struct nvme_resv_report_args *args``
  struct nvme_resv_report_args argument structure

**Description**

Returns a Reservation Status data structure to memory that describes the
registration and reservation status of a namespace. See the defintion for
the returned structure, :c:type:`struct nvme_reservation_status <nvme_reservation_status>`, for more details.

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_zns_mgmt_send_args

   Arguments for the NVMe ZNS Management Send command

**Definition**

::

  struct nvme_zns_mgmt_send_args {
    __u64 slba;
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_zns_send_action zsa;
    __u32 data_len;
    bool select_all;
    __u8 zsaso;
  };

**Members**

``slba``
  Starting logical block address

``result``
  The command completion result from CQE dword0

``data``
  Userspace address of the data

``args_size``
  Size of :c:type:`struct nvme_zns_mgmt_send_args <nvme_zns_mgmt_send_args>`

``fd``
  File descriptor of nvme device

``timeout``
  timeout in ms

``nsid``
  Namespace ID

``zsa``
  Zone send action

``data_len``
  Length of **data**

``select_all``
  Select all flag

``zsaso``
  Zone Send Action Specific Option



.. c:function:: int nvme_zns_mgmt_send (struct nvme_zns_mgmt_send_args *args)


**Parameters**

``struct nvme_zns_mgmt_send_args *args``
  :c:type:`struct nvme_zns_mgmt_send_args <nvme_zns_mgmt_send_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_zns_mgmt_recv_args

   Arguments for the NVMe ZNS Management Receive command

**Definition**

::

  struct nvme_zns_mgmt_recv_args {
    __u64 slba;
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    enum nvme_zns_recv_action zra;
    __u32 data_len;
    __u16 zrasf;
    bool zras_feat;
  };

**Members**

``slba``
  Starting logical block address

``result``
  The command completion result from CQE dword0

``data``
  Userspace address of the data

``args_size``
  Size of :c:type:`struct nvme_zns_mgmt_recv_args <nvme_zns_mgmt_recv_args>`

``fd``
  File descriptor of nvme device

``timeout``
  timeout in ms

``nsid``
  Namespace ID

``zra``
  zone receive action

``data_len``
  Length of **data**

``zrasf``
  Zone receive action specific field

``zras_feat``
  Zone receive action specific features



.. c:function:: int nvme_zns_mgmt_recv (struct nvme_zns_mgmt_recv_args *args)


**Parameters**

``struct nvme_zns_mgmt_recv_args *args``
  :c:type:`struct nvme_zns_mgmt_recv_args <nvme_zns_mgmt_recv_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.


.. c:function:: int nvme_zns_report_zones (int fd, __u32 nsid, __u64 slba, enum nvme_zns_report_options opts, bool extended, bool partial, __u32 data_len, void *data, __u32 timeout, __u32 *result)

   Return the list of zones

**Parameters**

``int fd``
  File descriptor of nvme device

``__u32 nsid``
  Namespace ID

``__u64 slba``
  Starting LBA

``enum nvme_zns_report_options opts``
  Reporting options

``bool extended``
  Extended report

``bool partial``
  Partial report requested

``__u32 data_len``
  Length of the data buffer

``void *data``
  Userspace address of the report zones data

``__u32 timeout``
  timeout in ms

``__u32 *result``
  The command completion result from CQE dword0

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_zns_append_args

   Arguments for the NVMe ZNS Append command

**Definition**

::

  struct nvme_zns_append_args {
    __u64 zslba;
    __u64 *result;
    void *data;
    void *metadata;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 nsid;
    __u32 ilbrt;
    __u32 data_len;
    __u32 metadata_len;
    __u16 nlb;
    __u16 control;
    __u16 lbat;
    __u16 lbatm;
  };

**Members**

``zslba``
  Zone start logical block address

``result``
  The command completion result from CQE dword0

``data``
  Userspace address of the data

``metadata``
  Userspace address of the metadata

``args_size``
  Size of :c:type:`struct nvme_zns_append_args <nvme_zns_append_args>`

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``nsid``
  Namespace ID

``ilbrt``
  Initial logical block reference tag

``data_len``
  Length of **data**

``metadata_len``
  Length of **metadata**

``nlb``
  Number of logical blocks

``control``

``lbat``
  Logical block application tag

``lbatm``
  Logical block application tag mask



.. c:function:: int nvme_zns_append (struct nvme_zns_append_args *args)

   Append data to a zone

**Parameters**

``struct nvme_zns_append_args *args``
  :c:type:`struct nvme_zns_append_args <nvme_zns_append_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.




.. c:struct:: nvme_dim_args

   Arguments for the Discovery Information Management (DIM) command

**Definition**

::

  struct nvme_dim_args {
    __u32 *result;
    void *data;
    int args_size;
    int fd;
    __u32 timeout;
    __u32 data_len;
    __u8 tas;
  };

**Members**

``result``
  Set on completion to the command's CQE DWORD 0 controller response.

``data``
  Pointer to the DIM data

``args_size``
  Length of the structure

``fd``
  File descriptor of nvme device

``timeout``
  Timeout in ms

``data_len``
  Length of **data**

``tas``
  Task field of the Command Dword 10 (cdw10)



.. c:function:: int nvme_dim_send (struct nvme_dim_args *args)

   Send a Discovery Information Management (DIM) command

**Parameters**

``struct nvme_dim_args *args``
  :c:type:`struct nvme_dim_args <nvme_dim_args>` argument structure

**Return**

The nvme command status if a response was received (see
:c:type:`enum nvme_status_field <nvme_status_field>`) or -1 with errno set otherwise.

