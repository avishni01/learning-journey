# Azure Database Migration Service (Classic) Pricing Guide

## Overview
Azure Database Migration Service (DMS) is a fully managed service designed to enable seamless migrations from multiple database sources to Azure data platforms with minimal downtime.

## Pricing Models

### 1. Compute Pricing
- Based on the total number of vCores used for the service
- Charged on a per-hour basis
- Different pricing tiers: Basic, Standard, Premium

### 2. Hybrid Benefit
- Allows you to use on-premises SQL Server licenses with Software Assurance to pay a reduced rate

## Detailed Pricing Tiers

Azure Database Migration Service (classic) offers three primary pricing tiers:

### 1. Basic Tier
- **vCore options**: 1, 2, 4 vCores
- **Use case**: Ideal for small to medium-sized migrations with minimal downtime requirements
- **Features**: 
  - Supports basic migrations
  - Limited to fewer concurrent migrations
- **Limitations**: 
  - Does not support some advanced features like continuous sync

### 2. Standard Tier
- **vCore options**: 2, 4, 8 vCores
- **Use case**: Suitable for most migration scenarios, including medium to large databases
- **Features**:
  - Supports more concurrent migrations than Basic tier
  - Includes features like continuous data sync for minimal downtime migrations

### 3. Premium Tier
- **vCore options**: 4, 8, 16, 32 vCores
- **Use case**: Designed for large-scale migrations and enterprises with high-performance requirements
- **Features**:
  - Supports the highest number of concurrent migrations
  - Offers the best performance for large and complex migrations
  - Includes all advanced features like continuous sync and multiple database migrations

### Key Considerations for Tier Selection:
1. **Database Size**: Larger databases typically require higher tier services.
2. **Migration Complexity**: More complex migrations (e.g., heterogeneous migrations) may require higher tiers.
3. **Downtime Tolerance**: If minimal downtime is crucial, consider Standard or Premium tiers for continuous sync capabilities.
4. **Number of Concurrent Migrations**: Higher tiers allow for more simultaneous migrations.
5. **Budget**: Balance your migration needs with your budget constraints.

### Pricing Notes:
- Prices are charged on a per-hour basis.
- You can change tiers during the migration process if needed.
- Specific pricing details can be found on the Azure pricing page and may vary by region.

## Best Practices

1. **Right-size your instances**: Choose the appropriate pricing tier based on your migration needs.
2. **Use the pricing calculator**: Estimate your costs before starting the migration process.
3. **Consider long-term vs short-term needs**: Balance between performance and cost.
4. **Monitor usage**: Regularly check your usage to optimize costs.
5. **Start small and scale up**: Begin with a lower tier and upgrade if needed, rather than overprovisioning.

## Use Cases

1. Migrating on-premises SQL Server to Azure SQL Database
2. Migrating MySQL to Azure Database for MySQL
3. Migrating PostgreSQL to Azure Database for PostgreSQL
4. Large-scale database migrations with minimal downtime

## Related Topics

- Azure SQL Database pricing
- Azure Database for MySQL pricing
- Azure Database for PostgreSQL pricing
- Azure Hybrid Benefit for SQL Server

## Important Exam Notes

- Understand the differences between pricing tiers and their use cases
- Know how to calculate potential costs for different migration scenarios
- Be familiar with the Azure Hybrid Benefit and its impact on pricing
- Understand how to optimize costs during and after migration
- Be able to recommend the appropriate tier based on migration requirements

## Relevant Documentation

- [Azure Database Migration Service pricing](https://azure.microsoft.com/en-us/pricing/details/database-migration/)
- [Azure Database Migration Service documentation](https://docs.microsoft.com/en-us/azure/dms/)
- [Azure Hybrid Benefit for SQL Server](https://azure.microsoft.com/en-us/pricing/hybrid-benefit/)

Remember to check the official Azure documentation for the most up-to-date information, as pricing and features may change over time.

---
## Migration performance
- Disable auto update and auto create statistics during migration.
- Partition tables and indexes, drop indexed views, and recreate them after migration.
- Consider migrating rarely queried historical data to a separate database in Azure SQL Database, and query it using elastic queries.

- You can fine-tune the behavior of the Data Migration Assistant by changing configuration values in the dma.exe.config file.
- Avoid installing and running the Data Migration Assistant directly on the SQL Server host machine.






# TODO
1. https://learn.microsoft.com/en-us/training/modules/migrate-sql-workloads-azure-sql-databases/2-choose-right-sql-database-option-azure
2. https://learn.microsoft.com/en-us/data-migration/sql-server/database/overview

---
- distributed availability groups (migration)
- Microsoft SQL Server Data Tools (SSDT) database project
- TRUSTWORTHY property and the CLR property
- Encrypt a copy of the master key by using the service master key
- Create contained database users in your database mapped to Azure AD identities
- Authenticate database users by using Active Directory credentials
- create a contained database user in DB who will use Azure Active Directory (Azure AD) for authentication
- You need to encrypt the Salary column.
- record whenever users query a PII table.
- Enabling auditing on the database
- Microsoft.Sql resource provider
- Query Store transitioning to a read-only state
- sys.resource_stats sys
- retrieve the resource usage of db from the last week
- identify the cause of the performance issues
- out-of-date statistics and frequent blocking queries
- sys.dm_pdw_nodes_tran_locks and of sys.dm_tran_locks
- force query plan
- Query Store
- Query Performance Insight
- Last_Query_Plan_Stats
- SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN=ON)
- SET AUTOMATIC_TUNING=AUTO
- prevent reading queries from blocking queries that are trying to write to the database
- READ_COMMITTED_SNAPSHOT to ON
- sys.database_scoped_configurations
- OPTIMIZE_FOR_AD_HOC_WORKLOADS
- receives an email alert if a job fails
- Azure Policy Initiative assignment.
- apply 20 built-in Azure Policy definitions to all new and existing Azure SQL Database deployments in an Azure subscription
-  Cloud witness
-  Azure Basic Load Balancer
-  General Purpose service tier and failover groups
-  Business Critical service tier and Availability Zones.
-  RESTORE Transact-SQL command and the REPLACE option.