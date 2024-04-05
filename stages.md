# Stages

There are 2 types of stages

- Unnamed
    Automatically defined no need to setup
    Not Viewable objects on stages tab of webUI

  - Table
    - To Access Table stage use following cmd:
        ```sql
        @%[table_name]
        ```
  - User
    - To Access Table stage use following cmd:
        ```sql
        @%[table_name]
        ```
- Named
  - Internal
  - External 
  
## How to list stage

- list stages;
- show stages;


## PUT
Uploads data files from a local file system to one of the following Snowflake stages:

- A named internal stage.

- A specified table’s internal stage.

- The current user’s internal stage.
  
You can load staged files into a table using the [COPY INTO \<table>](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table) command.

### Syntax

```sql
PUT file://<path_to_file>/<filename> internalStage
    [ PARALLEL = <integer> ]
    [ AUTO_COMPRESS = TRUE | FALSE ]
    [ SOURCE_COMPRESSION = AUTO_DETECT | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE ]
    [ OVERWRITE = TRUE | FALSE ]
```

**Linux/macOS** You must include the initial forward slash in the path. For example, for a file named load use file:///tmp/load.

`AUTO_COMPRESS = TRUE | FALSE`

Specifies whether Snowflake uses gzip to compress files during upload:

- `TRUE` : Snowflake compresses the files (if they are not already compressed).

- `FALSE` : Snowflake doesn’t compress the files.

This option does not support other compression types. To use a different compression type, compress the file separately before executing the PUT command. Then, identify the compression type using the `SOURCE_COMPRESSION` option.

Ensure your local folder has sufficient space for Snowflake to compress the data files before staging them. If necessary, set the TEMP, TMPDIR or TMP environment variable in your operating system to point to a local folder that contains additional free space.

Default: `TRUE`

`SOURCE_COMPRESSION = AUTO_DETECT | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE`

Specifies the method of compression used on already-compressed files that are being staged:

| Supported Values | Notes |
|--|--|
| AUTO_DETECT | Compression algorithm detected automatically, except for Brotli-compressed files, which cannot currently be detected automatically. If loading Brotli-compressed files, explicitly use BROTLI instead of AUTO_DETECT. |
| GZIP |
| BZ2 | 
| etc... |

Default: `AUTO_DETECT`



` OVERWRITE = TRUE | FALSE `
Specifies whether Snowflake overwrites an existing file with the same name during upload:

- `TRUE` : An existing file with the same name is overwritten.

- `FALSE` : An existing file with the same name is not overwritten.

If attempts to PUT a file fail because a file with the same name exists in the target stage, the following options are available:

- Load the data from the existing file into one or more tables, and remove the file from the stage. Then PUT a file with new or updated data to the stage.

- Rename the local file, and then attempt the PUT operation again.

- Set `OVERWRITE = TRUE` in the PUT statement. Do this only if it is actually safe to overwrite a file with data that might not yet have been loaded into Snowflake.

Note that if your Snowflake account is hosted on Google Cloud Platform, PUT statements do not recognize when the **OVERWRITE** parameter is set to TRUE. A PUT operation **_always_** overwrites any existing files in the target stage with the local files you are uploading.

The following clients support the OVERWRITE option for Snowflake accounts hosted on Amazon Web Services or Microsoft Azure:

- SnowSQL
- Snowflake ODBC Driver
- Snowflake JDBC Driver 
- Snowflake Connector for Python
- Supported values: TRUE, FALSE.

Default: `FALSE`.