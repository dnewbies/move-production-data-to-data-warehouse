# Install pgloader:
Installation guide:
https://github.com/dimitri/pgloader/blob/master/INSTALL.md

# Create file migrate_mysql.load

LOAD DATABASE
     FROM      mysql://team5:923494g9uii8WV^V819c@118.69.73.196:3333/svcdb
     INTO postgresql://postgres:123456@61.28.227.201/dw


 WITH include drop, create tables, create indexes, reset sequences,
      workers = 8, concurrency = 1

  SET maintenance_work_mem to '256MB',
      work_mem to '24MB',
      search_path to 'prod'

  ALTER TABLE NAMES MATCHING 'product' RENAME TO 'product_m'
  ALTER TABLE NAMES MATCHING 'selling_order' RENAME TO 'selling_order_m'
  ALTER TABLE NAMES MATCHING 'selling_store' RENAME TO 'selling_store_m'
  ALTER TABLE NAMES MATCHING 'user' RENAME TO 'user_m'
  ALTER TABLE NAMES MATCHING 'video' RENAME TO 'video_m'

 BEFORE LOAD DO
 $$ create schema if not exists prod; $$
 AFTER LOAD DO
 $$ ALTER TABLE IF EXISTS prod.product RENAME TO  product_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.product_m RENAME TO product; $$,
 $$ DROP TABLE IF EXISTS prod.product_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.selling_order RENAME TO selling_order_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.selling_order_m RENAME TO selling_order; $$,
 $$ DROP TABLE IF EXISTS prod.selling_order_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.selling_store RENAME TO selling_store_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.selling_store_m RENAME TO selling_store; $$,
 $$ DROP TABLE IF EXISTS prod.selling_store_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.video RENAME TO video_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.video_m RENAME TO video; $$,
 $$ DROP TABLE IF EXISTS prod.video_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.user RENAME TO user_bk; $$,
 $$ ALTER TABLE IF EXISTS prod.user_m RENAME TO "user"; $$,
 $$ DROP TABLE IF EXISTS prod.user_bk; $$;
 
 # Create cron job run hourly
 Go to folder /etct/cron/cron.hourly/
 Create file migreate_prod
   #!/bin/bash
  pgloader migrate_mysql.load
  
  ----- DONE ------
