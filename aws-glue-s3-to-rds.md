# aws glue s3 to rds (mysql)

### Create database in AWG Glue
in example i have create 2 database
- import-csv: database to detect csv from S3
- hr-mysql: database to detect table from RDS
### Create Data Connection 
mục đích để tạo connection tới Rds Mysql, lúc sau Glue nó sẻ dùng nó để commit data vào mysql
những hạng mục setting cần lưu ý
- JDBC URL: path jdbc của Rds, ex: `jdbc:mysql://xxx.yyy.ap-northeast-1.rds.amazonaws.com:3306/hr`, có thể thay đổi driver mysql thaành những database khác như:
    - mysql for MySQL
    - postgresql for PostgreSQL
    - oracle:thin for Oracle Thin
    - oracle:oci for Oracle OCI
    - oracle:oci8 for Oracle OCI 8
    - oracle:kprb for Oracle KPRB
    - sqlserver for SQL Server
- Network Option: bởi vì stack của Glue cần Instance ( PySpark ) để xử lý và lưu data vào mysql, nên phải config VPC, SubNet, Security Group làm sao cho nó có thể access được database
    - VPC: cùng vpc với DB
    - Subnet: cùng subnet với DB
    - Security Group : allow port inbound cho Glue 
    sau khi tạo data connection mình có thể test connection, nếu gặp lỗi S3 End Point thì cần phải tạo End Point cho VPC và Subnet trên. <br>
    
    vào VPC > EndPoint để tạo endpoint, cần setting như sau:

        - VPC: chọn VPC như ở step setting Network Option
        - ServiceName: com.amazonaws.ap-northeast-1.s3
        - EndPoint Type: Gateway
        - Policy: Full access
### Create 2 crawler and attached it to 2 database above
tại sao phải tạo 2 crawler mà k phải là 2 table trong Glue, vì Glue Table chỉ support S3, Kenises, Kafka, ở đay mình dùng Mysql


