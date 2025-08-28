# Database Fixing

The Enhanced TEx database fixing feature provides comprehensive tools to maintain database consistency and ensure proper user data synchronization across all database sources.

## Overview

The database fixing system addresses common issues that can occur during data collection:

- **Missing user entries** in group-specific user databases
- **Incomplete user data** (missing names, usernames)
- **Inconsistencies** between message databases and user databases
- **Data synchronization** across multiple database sources

## Database Sources

The enhanced database fixer utilizes multiple database sources to ensure complete user data:

### 1. Message Database (`message.db`)
- **Location**: `{group_id}/message.db`
- **Purpose**: Contains message data with user IDs
- **Used for**: Identifying which users have sent messages

### 2. Group User Database (`user.db`)
- **Location**: `{group_id}/user.db`
- **Purpose**: Group-specific user profiles
- **Used for**: Storing user data for the specific group

### 3. User Index Database (`user_index.db`)
- **Location**: `UserProfile/user_index.db`
- **Purpose**: Global user profile index
- **Used for**: Quick user lookups and cross-referencing

### 4. User Profiles Database (`user_profiles.db`)
- **Location**: `UserProfile/user_profiles.db`
- **Purpose**: Global user profile database with detailed information
- **Used for**: Complete user data including profile pictures, bio, etc.

## Commands

### Basic Database Fixing

```bash
# Fix all groups
python -m TEx fix_database --config config.ini

# Fix specific group
python -m TEx fix_database --config config.ini --group_id 12345

# Check for inconsistencies without fixing
python -m TEx fix_database --config config.ini --check-only
```

### Command Options

| Option | Description | Example |
|--------|-------------|---------|
| `--config` | Configuration file path | `--config config.ini` |
| `--group_id` | Specific group ID to fix | `--group_id 12345` |
| `--check-only` | Check consistency without fixing | `--check-only` |

## How It Works

### 1. Consistency Check

The system performs a comprehensive consistency check:

```bash
python -m TEx fix_database --config config.ini --check-only
```

**Output Example:**
```
🔧 Starting Enhanced Database Fixer Module
🔍 Performing comprehensive database consistency check...
📊 Comprehensive Database Consistency Check Results:
   Groups checked: 3
   Inconsistent groups: 1
   Total missing users: 15
   Users with incomplete data: 8
   Group Details:
     Group 12345: ✅ Consistent (150/150 users)
     Group 67890: ❌ Inconsistent (15 missing users)
     Group 11111: ⚠️  Incomplete Data (8 users need updates)
       Database sources: user_index.db, user_profiles.db
```

### 2. Database Fixing

When fixing is enabled, the system:

1. **Identifies missing users** from message databases
2. **Retrieves user data** from available sources in order of preference:
   - `user_profiles.db` (most complete)
   - `user_index.db` (basic info)
   - Creates placeholder entries if no data found
3. **Updates incomplete user data** with missing names/usernames
4. **Synchronizes data** across all database sources

```bash
python -m TEx fix_database --config config.ini
```

**Output Example:**
```
🔧 Starting Enhanced Database Fixer Module
🔧 Executing comprehensive database fixes...
🔧 Comprehensive Database Fix Results:
   Groups processed: 3
   Groups fixed: 2
   Users synced: 15
   Users updated: 8
   Database sources used: 2
   Group Details:
     Group 12345: ℹ️  No changes needed (150/150 users)
     Group 67890: ✅ Fixed (15 synced, 0 updated)
       Data sources: user_index.db, user_profiles.db
     Group 11111: ✅ Fixed (0 synced, 8 updated)
       Data sources: user_index.db, user_profiles.db
```

## User Data Priority

The system retrieves user data in the following priority order:

### 1. User Profiles Database (`user_profiles.db`)
- **Most complete data**
- Includes: first_name, last_name, username, phone_number, bio, profile_photo, etc.
- **Used when**: Complete user profile is needed

### 2. User Index Database (`user_index.db`)
- **Basic user information**
- Includes: first_name, last_name, username, basic flags
- **Used when**: Basic user data is sufficient

### 3. Placeholder Creation
- **Fallback option**
- Creates minimal user entry with user ID only
- **Used when**: No user data found in any source

## User Display Name Optimization

The database fixer ensures proper user display name formatting:

### Display Name Format
- **Username available**: `username (username)`
- **Full name available**: `First Last (user_id)`
- **No name data**: `User 123456789 (123456789)`

### Example Output
```json
{
  "user_display_names": {
    123456789: "johndoe (johndoe)",
    987654321: "Jane Smith (987654321)",
    555666777: "User 555666777 (555666777)"
  }
}
```

## Database Consistency Status

The system reports different consistency statuses:

### ✅ Consistent
- All users from messages exist in user database
- All users have complete data (names or usernames)
- No action needed

### ❌ Inconsistent
- Missing users found in message database
- Users need to be synced from other sources

### ⚠️ Incomplete Data
- All users exist but some lack complete information
- Users need data updates from other sources

### 🔧 Missing Database
- Required database files are missing
- Cannot perform consistency check

## Advanced Usage

### Fix Specific Group with Verbose Output

```bash
# Enable debug logging for detailed information
export TEX_LOG_LEVEL=DEBUG
python -m TEx fix_database --config config.ini --group_id 12345
```

### Batch Processing Multiple Groups

```bash
# Fix all groups in sequence
python -m TEx fix_database --config config.ini

# Check all groups first, then fix only inconsistent ones
python -m TEx fix_database --config config.ini --check-only
# Review output and fix specific groups as needed
python -m TEx fix_database --config config.ini --group_id 67890
```

## Integration with Analysis

The database fixer is automatically called during analysis operations:

```bash
# Analysis will automatically fix databases before processing
python -m TEx analyze --config config.ini --group_id 12345 --visual-data
```

**Automatic Fixing Process:**
1. Analysis module detects database inconsistencies
2. Automatically calls database fixer
3. Fixes user data using all available sources
4. Proceeds with analysis using complete data

## Troubleshooting

### Common Issues

#### 1. "Database not found" Error
```bash
Error: Database file not found: /path/to/group/12345/message.db
```

**Solution:**
- Ensure the group has been downloaded first
- Check if the data path is correct in config.ini
- Verify group ID exists in your data directory

#### 2. "Permission denied" Error
```bash
Error: Permission denied when accessing database
```

**Solution:**
- Check file permissions on database files
- Ensure write access to the data directory
- Run with appropriate user permissions

#### 3. "No users synced" Warning
```bash
Warning: No users were synced for group 12345
```

**Possible causes:**
- All users already exist in the database
- No user data available in source databases
- Group has no messages (no users to sync)

### Debug Mode

Enable debug logging for detailed troubleshooting:

```bash
# Set debug logging
export TEX_LOG_LEVEL=DEBUG

# Run with verbose output
python -m TEx fix_database --config config.ini --group_id 12345
```

## Performance Considerations

### Large Datasets
- **Memory usage**: Database fixer loads user data into memory
- **Processing time**: Large groups may take several minutes
- **Recommendation**: Process groups individually for large datasets

### Database Size
- **Storage impact**: Minimal - only adds missing user entries
- **Performance impact**: Improves query performance with complete data
- **Backup recommendation**: Backup databases before major fixes

## Best Practices

### 1. Regular Maintenance
```bash
# Run consistency checks weekly
python -m TEx fix_database --config config.ini --check-only

# Fix inconsistencies as needed
python -m TEx fix_database --config config.ini
```

### 2. Before Analysis
```bash
# Always fix databases before major analysis
python -m TEx fix_database --config config.ini
python -m TEx analyze --config config.ini --group_id 12345 --visual-data
```

### 3. Data Backup
```bash
# Backup databases before major operations
cp -r DataPath/ DataPath_backup_$(date +%Y%m%d)
```

### 4. Monitoring
- Check consistency reports regularly
- Monitor database sizes and performance
- Review user data completeness

## API Reference

### DatabaseFixer Class

```python
from TEx.utils.database_fixer import DatabaseFixer

# Initialize fixer
fixer = DatabaseFixer(data_path)

# Check consistency
result = fixer.check_database_consistency_comprehensive(group_id)

# Fix databases
result = fixer.fix_databases_comprehensive(group_id)
```

### Result Structure

```python
{
    'groups_processed': 3,
    'groups_fixed': 2,
    'users_synced': 15,
    'users_updated': 8,
    'sources_used': 2,
    'details': {
        '12345': {
            'group_id': 12345,
            'users_synced': 0,
            'users_updated': 0,
            'status': 'no_changes_needed'
        }
    },
    'errors': []
}
```

---

**Database Fixing** - Maintain database consistency and ensure complete user data across all sources

*Last updated: August 2025*