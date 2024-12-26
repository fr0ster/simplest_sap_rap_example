# Behaviour Implementation definition

## Behaviour Implementation for CDS Criticality Levels Interface
<a name="z##_i_criticality_levels_"></a>
Z##_I_CRITICALITY_LEVELS_####

```ABAP
managed implementation in class zbp_##_i_crtlty_levels_#### unique;
strict ( 2 );

define behavior for Z##_I_CRITICALITY_LEVELS_#### alias CriticalityLevels
persistent table z##_d_crtly_####
lock master
authorization master ( instance )
etag master LocalChangedTime
{
  create;
  update;
  delete;

  field ( readonly ) Id;

  mapping for z##_d_crtly_####
    {
      Id               = id;
      Treashhold1      = treashhold1;
      Treashhold2      = treashhold2;
      Treashhold3      = treashhold3;
      Treashhold4      = treashhold4;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = changed_time;
      LocalChangedTime = local_changed_time;
    }
}
```