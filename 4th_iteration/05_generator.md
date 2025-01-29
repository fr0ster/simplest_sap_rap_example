# Class for data generation
<a name="z##_cl_generate_data_"></a>
z##_cl_generate_data_####

```ABAP
CLASS z##_cl_generate_data_#### DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    INTERFACES if_oo_adt_classrun .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.


CLASS z##_cl_generate_data_#### IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    DATA lt_prod_grs TYPE TABLE OF z##_d_pg_####.
    DATA lt_phases   TYPE TABLE OF z##_d_phase_####.
    DATA lt_countries  TYPE TABLE OF z##_d_ctry_####.
    DATA lt_uom      TYPE TABLE OF z##_d_uom_####.
    DATA lt_critically TYPE TABLE OF z##_d_crtly_####.

    " PRODUCT GROUPS
    " fill internal table (itab)

    lt_prod_grs = VALUE #(
        ( pgid     = '1'
          pgname   = 'Microwave'
          imageurl = 'https://png.pngtree.com/png-clipart/20190517/original/pngtree-vector-microwave-oven-icon-png-image_4015182.jpg' )
        ( pgid  = '2' pgname = 'Coffee Machine' imageurl = 'https://icon-library.com/images/coffee-maker-icon/coffee-maker-icon-13.jpg' )
        ( pgid     = '3'
          pgname   = 'Waffle Iron'
          imageurl = 'https://previews.123rf.com/images/anatolir/anatolir1810/anatolir181004863/110698658-waffle-maker-icon-outline-style.jpg' )
        ( pgid     = '4'
          pgname   = 'Blender'
          imageurl = 'https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcRSLnFTOSs5ZV0d8pwhPzs4KANsvq1oZ7hyrg&usqp=CAU' )
        ( pgid     = '5'
          pgname   = 'Co##er'
          imageurl = 'https://st4.depositphotos.com/18657574/22404/v/1600/depositphotos_224044856-stock-illustration-stove-concept-vector-linear-icon.jpg' ) ).

    " Delete the possible entries in the database table - in case it was already filled
    DELETE FROM z##_d_pg_####.
    " insert the new table entries
    INSERT z##_d_pg_#### FROM TABLE @lt_prod_grs.

    " check the result
    SELECT * FROM z##_d_pg_#### INTO TABLE @lt_prod_grs.
    out->write( sy-dbcnt ).
    out->write( 'product groups data inserted successfully!' ).

    " PHASES
    " fill internal table (itab)
    lt_phases = VALUE #( ( phaseid  = '1' phase = 'PLAN' )
                         ( phaseid  = '2' phase = 'DEV' )
                         ( phaseid  = '3' phase = 'PROD' )
                         ( phaseid  = '4' phase = 'OUT' ) ).

    " Delete the possible entries in the database table - in case it was already filled
    DELETE FROM z##_d_phase_####.
    " insert the new table entries
    INSERT z##_d_phase_#### FROM TABLE @lt_phases.

    " check the result
    SELECT * FROM z##_d_phase_#### INTO TABLE @lt_phases.
    out->write( sy-dbcnt ).
    out->write( 'phases data inserted successfully!' ).

    " MARKETS
    " fill internal table (itab)
    lt_countries = VALUE #(
        ( land     = '1'
          name     = 'Russia'
          code     = 'RU'
          imageurl = 'https://cdn.webshopapp.com/shops/94414/files/54940426/russia-flag-image-free-download.jpg' )
        ( land  = '2'  name = 'Belarus'         code = 'RU' imageurl = 'https://cdn.countryflags.com/thumbs/belarus/flag-400.png' )
        ( land     = '3'
          name     = 'United Kingdom'
          code     = 'EN'
          imageurl = 'https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Flag_of_the_United_Kingdom.svg/640px-Flag_of_the_United_Kingdom.svg.png' )
        ( land     = '4'
          name     = 'France'
          code     = 'FR'
          imageurl = 'https://cdn.webshopapp.com/shops/94414/files/54002660/france-flag-image-free-download.jpg' )
        ( land     = '5'
          name     = 'Germany'
          code     = 'DE'
          imageurl = 'https://cdn.webshopapp.com/shops/94414/files/54006402/germany-flag-image-free-download.jpg' )
        ( land  = '6'  name = 'Italy'           code = 'IT' imageurl = 'https://cdn.countryflags.com/thumbs/italy/flag-400.png' )
        ( land     = '7'
          name     = 'USA'
          code     = 'EN'
          imageurl = 'https://cdn.webshopapp.com/shops/94414/files/54958906/the-united-states-flag-image-free-download.jpg' )
        ( land  = '8'  name = 'Japan'           code = 'EN' imageurl = 'https://image.freepik.com/free-vector/illustration-japan-flag_53876-27128.jpg' )
        ( land     = '9'
          name     = 'Poland'
          code     = 'EN'
          imageurl = 'https://cdn.webshopapp.com/shops/94414/files/54940016/poland-flag-image-free-download.jpg' )
        ( land     = '10'
          name     = 'Spain'
          code     = 'ES'
          imageurl = 'https://cdn.webshopapp.com/shops/94414/files/54940016/poland-flag-image-free-download.jpg' ) ).

    " Delete the possible entries in the database table - in case it was already filled
    DELETE FROM z##_d_ctry_####.
    " insert the new table entries
    INSERT z##_d_ctry_#### FROM TABLE @lt_countries.

    " check the result
    SELECT * FROM z##_d_ctry_#### INTO TABLE @lt_countries.
    out->write( sy-dbcnt ).
    out->write( 'markets data inserted successfully!' ).

    " UOM
    " fill internal table (itab)
    lt_uom = VALUE #( dimid = 'LENGTH'
                      ( msehi = 'CM'  isocode = 'CMT' )
                      ( msehi = 'DM'  isocode = 'DMT' )
                      ( msehi = 'FT'  isocode = 'FOT' )
                      ( msehi = 'IN'  isocode = 'INH' )
                      ( msehi = 'KM'  isocode = 'KMT' )
                      ( msehi = 'M'   isocode = 'MTR' )
                      ( msehi = 'MI'  isocode = 'SMI' )
                      ( msehi = 'MIM' isocode = '4H' )
                      ( msehi = 'MM'  isocode = 'MMT' )
                      ( msehi = 'NAM' isocode = 'C45' )
                      ( msehi = 'YD'  isocode = 'YRD' ) ).

    " Delete the possible entries in the database table - in case it was already filled
    DELETE FROM z##_d_uom_####.
    " insert the new table entries
    INSERT z##_d_uom_#### FROM TABLE @lt_uom.

    " check the result
    SELECT * FROM z##_d_uom_#### INTO TABLE @lt_uom.
    out->write( sy-dbcnt ).
    out->write( 'uom data inserted successfully!' ).

    " Price Critically
    " fill internal table (itab)
    lt_critically = VALUE #( id = 'PriceCritically' ( treashhold4 = 700 treashhold3 = 1200 treashhold2 = 1800 treashhold1 = 2000 ) ).

    " Delete the possible entries in the database table - in case it was already filled
    DELETE FROM z##_d_crtly_####.
    " insert the new table entries
    INSERT z##_d_crtly_#### FROM TABLE @lt_critically.

    " check the result
    SELECT * FROM z##_d_crtly_#### INTO TABLE @lt_critically.
    out->write( sy-dbcnt ).
    out->write( 'uom data inserted successfully!' ).
  ENDMETHOD.
ENDCLASS.
```