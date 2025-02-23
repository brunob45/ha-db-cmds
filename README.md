# ha-db-cmds
Useful commands to handle the Home Assistant Database

> [!IMPORTANT]
> Always shutdown Home Assistant before editing the database.

## List all unused state_attributes

I had an issue with Tuya Zigbee devices connected via zigbee2mqtt, where the state_attributes would change on every update, increasing the size of the database dramatically. I used this command to find all unused attributes.

```sql
SELECT attributes_id
FROM state_attributes
WHERE attributes_id NOT IN (
    SELECT attributes_id
    FROM states
)
```

Then, I used this command to delete all the unused attributes.

```sql
DELETE
FROM state_attributes
WHERE attributes_id NOT IN (
    SELECT attributes_id
    FROM states
)
```

## Find the most used attributes for a given state

I don't remember why this was useful

```sql
SELECT COUNT(*) as count, states.attributes_id, shared_attrs
FROM states
JOIN state_attributes ON states.attributes_id=state_attributes.attributes_id
WHERE states.metadata_id=165
GROUP BY states.attributes_id
ORDER BY count DESC
```