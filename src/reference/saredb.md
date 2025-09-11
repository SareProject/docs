# SareDB in SARE-CLI

`SareDB` is a **command-line specific component** of SARE. It is **not part of the core SARE library (`sare-lib`)** and exists purely to facilitate local management of keys and recipients in SARE CLI.

> ⚠️ Note: `SareDB` is in an early stage and may **evolve significantly** in future versions.

## Purpose

`SareDB` is designed to act as a **lightweight, local database** for storing:

1. **Master key associations**
   
    - Links a master key identifier to associated keys and revocation certificates.
      
    - Each association is stored as a `SareDBAssociatedKey` object containing:
      
        - `public_key_id` — the associated public key ID
          
        - `revocation_certificate_id` — the linked revocation certificate ID
    
2. **Recipients**
   
    - Tracks known recipients to simplify encryption workflows.
      
    - Stored as `SareDBRecipient` objects with:
      
        - `fullchain_fingerprint` — the recipient’s key fingerprint
          
        - Optional `comment`
          
        - `date_added` timestamp
          

## Structure and Storage

- Internally, `SareDB` uses two `HashMap`s:
  
    - `key_associations`: maps a master key ID to its associated key information.
      
    - `recipients`: maps a recipient identifier to recipient metadata.
    
- The database is serialized and stored as a **JSON file** in the SARE directory, making it human-readable and easy to inspect.
  
- `version` field allows for future upgrades or migrations to the database format.
  

## Functionality

`SareDB` provides the following operations:

1. **Adding and updating keys**
   
    - `add_key_association(master_key_id, associated_key)` inserts or updates a master key association.
    
2. **Adding recipients**
   
    - `add_recipient(recipient_id, recipient)` adds a new recipient to the database.
    
3. **Retrieving information**
   
    - `get_key_association(master_key_id)` returns an associated key if it exists.
      
    - `list_recipients()` returns all stored recipients.
    
4. **Persistence**
   
    - `import_from_json_file()` loads the database from JSON or returns an empty database if the file does not exist.
      
    - `save_to_json_file()` writes the current database to JSON for persistence between CLI sessions.
      

## Notes

- `SareDB` is primarily intended for **local, per-user storage**. It is not a cryptographically secure or distributed database.
  
- Since SARE CLI is still under active development, the structure, storage format, and supported fields may change. Users should not rely on `SareDB` as a stable long-term API.