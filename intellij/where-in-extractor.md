# Where IN extractor

Extracts the query result to a `WHERE IN (<comma_separated_rows>)` statement.

Should be placed in folder along with other extractors which are located in `Scratches and Consoles | Extensions | Database Tools and SQL | data | extractors.`

As per the recommended naming convention in the docs, the name would be `SQL-Where-In.sql.groovy`

```groovy
SEP = ", "
QUOTE     = "\'"
STRING_PREFIX = DIALECT.getDbms().isMicrosoft() ? "N" : ""
NEWLINE   = System.getProperty("line.separator")

KEYWORDS_LOWERCASE = com.intellij.database.util.DbSqlUtil.areKeywordsLowerCase(PROJECT)
KW_WHERE = KEYWORDS_LOWERCASE ? "where " : "WHERE "
KW_VALUES = KEYWORDS_LOWERCASE ? "in" : "IN"
KW_NULL = KEYWORDS_LOWERCASE ? "null" : "NULL"

begin = true

def record(columns, dataRow) {
    if (columns.size() != 1) {
        throw new Error("Extractor operates only on one column.");
    }

    if (begin) {
        OUT.append(KW_WHERE)

        columns.eachWithIndex { column, idx ->
            OUT.append(column.name()).append(idx != columns.size() - 1 ? SEP : "")
        }

        OUT.append(NEWLINE)
        OUT.append(KW_VALUES).append(" (")
        begin = false
    }
    else {
        OUT.append(",").append(NEWLINE)
        OUT.append("    ")
    }

    columns.eachWithIndex { column, idx ->
        def value = dataRow.value(column)
        def stringValue = value == null ? KW_NULL : FORMATTER.formatValue(value, column)
        def isStringLiteral = value != null && FORMATTER.isStringLiteral(value, column)
        if (isStringLiteral && DIALECT.getDbms().isMysql()) stringValue = stringValue.replace("\\", "\\\\")
        OUT.append(isStringLiteral ? (STRING_PREFIX + QUOTE) : "")
                .append(stringValue ? stringValue.replace(QUOTE, QUOTE + QUOTE) : stringValue)
                .append(isStringLiteral ? QUOTE : "")
                .append(idx != columns.size() - 1 ? SEP : "")
    }
}

ROWS.each { row -> record(COLUMNS, row) }
OUT.append(");")
```

[Reference documentation](https://www.jetbrains.com/help/idea/data-extractors.html)
