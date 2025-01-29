# Notes

## Seventh Iteration: Draft Handling

In this iteration, we enhance the User interface.

---

### Key Objectives:
1. Modificate **[Behavior Definition CDS Interface](./06_behavior_definition.md#z##_i_product_)**
2. Modificate **[Behavior Definition CDS Projection Transactional Interface](./06_behavior_definition.md#z##_ci_product_)**.
2. Modificate **[Behavior Definition CDS Projection Transactional Query](./06_behavior_definition.md#z##_c_product_)**.

---

### Steps:
1. Modificate **[Behavior Definition CDS Interface](./01_cds.md#z##_i_product_)**
    ```ABAP
    managed with additional save implementation in class zbp_##_i_product_#### unique;
    strict ( 2 );
    with draft;

    define behavior for Z##_I_PRODUCT_#### alias Product
    persistent table z##_d_prod_####
    draft table z##_dt_prod_####
    lock master
    total etag LocalChangedTime
    authorization master ( instance )
    etag master LocalChangedTime
    {
     " Part of code was skipped

    association _Market { create; with draft; }

     " Part of code was skipped

    draft action Edit;
    draft action Activate optimized;
    draft action Discard;
    draft action Resume;
    draft determine action Prepare
    {
        validation checkHeight;
        validation checkDepth;
        validation checkWidth;
        validation Order~checkQuantity;
        validation Order~checkNetamount;
        validation Order~checkGrossamount;
    }

     " Part of code was skipped
    }

    define behavior for Z##_I_MARKET_#### alias Market
    persistent table z##_d_mrkt_####
    draft table z##_dt_mrkt_####
    lock dependent by _Product
    authorization dependent by _Product
    etag master LocalChangedTime
    {
     " Part of code was skipped

    association _Product { with draft; }
    association _Orders { create; with draft; }

     " Part of code was skipped
    }

    define behavior for Z##_I_ORDER_#### alias Order
    persistent table z##_d_order_####
    draft table z##_dt_ordr_####
    lock dependent by _Product
    authorization dependent by _Product
    etag master LocalChangedTime
    {
     " Part of code was skipped

    association _Product { with draft; }
    association _Market { with draft; }

     " Part of code was skipped
    }
    ```

2. **[Add auxillary field to Projection Transactional Interface](./02_cds.md#z##_ci_product_)**:
    ```ABAP
    interface;
    use draft;

    define behavior for Z##_CI_PRODUCT_#### alias Product
    {
     " Part of code was skipped

    use action Prepare;
    use action Edit;
    use action Activate;
    use action Discard;
    use action Resume;

    use association _Market { create; with draft; }
     " Part of code was skipped
    }

    define behavior for Z##_CI_MARKET_#### alias Market
    {
     " Part of code was skipped

    use association _Product {  with draft; }
    use association _Orders { create; with draft; }
    }

    define behavior for Z##_CI_ORDER_#### alias Order
    {
     " Part of code was skipped

    use association _Product {  with draft; }
    use association _Market {  with draft; }
    }
    ```

3. **[Add auxillary field to Projection Transactional Query](./02_cds.md#z##_c_product_)**:
    ```ABAP
    projection implementation in class zbp_##_c_product_#### unique;
    strict ( 2 );
    use draft;

    define behavior for Z##_C_PRODUCT_#### alias Product
    {
     " Part of code was skipped

    use action Prepare;
    use action Edit;
    use action Activate;
    use action Discard;
    use action Resume;

    use association _Market { create; with draft; }
     " Part of code was skipped
    }

    define behavior for Z##_C_MARKET_#### alias Market
    {
     " Part of code was skipped

    use association _Product {  with draft; }
    use association _Orders { create; with draft; }
    }
    define behavior for Z##_C_ORDER_#### alias Order
    {
     " Part of code was skipped

    use association _Product {  with draft; }
    use association _Market {  with draft; }
    }
    ```

---

### Final Steps:

- Activate all updated BDEFs.
- Test the updated services in the Fiori application to ensure proper integration and functionality.

---

### Possible Issues and Solutions:

---

### Summary:
This iteration enabled Draft Handling.