# sitekick-entities
Entity definitions for the sitekick platform. The entities are defined by defining a yaml file with these ingredients:
* `key-names`: Additional names for the key of the entity. The default name is `key`, but any of the synonyms will work. List of names.
* `properties`: Any properties which are of interest for the entity class, NOT the instances. No applications yet.
* `attributes`: The attributes for the entity. Structure is identical to that of the connectors. Most used type is JsonStrInt, but any type may be used.
* `relations`: Relations from the specified entity to other entities. Can be defined in one place; the from-to is inverse to the to-from of course. 
  * identification of the target entity: `<<client>>/<<domain>>/<<entity>>`
    * name: the labels for the from and to names. If not supplied, will be derived from the entity names.
    * cardinality: specification of the number of items on each side of the relation.

An example definition:
```yaml
key-names:  # Additional names for the key of the entity. The default name is `key`, but any of the synonyms will work.
  - domain
  - name
# Rationale: most entities have a strong natural key name like `address` (ip), `domain` (domain) etc.

properties:  # Any properties which are of interest about the entity, NOT the instances. No applications yet.
  foo:
    bar: 1
    baz: 42

attributes:  # The attributes for the entity. Most used type is JsonStrInt, but any type may be used.
  type: JsonStrInt
  index:
    tld: << functions.hosting.get_tld(self.key) >> (str)  # Automatic calculation
    sld: << functions.hosting.get_sld(self.key) >> (str)  # Automatic calculation

relations:  # Relations from this entity to other entities. 
  sitekick/hosting/platform:
    name:
      from: domains  # Can be derived from 'sitekick/hosting/domain' and 'cardinality.from'
      to: platform  # Can be derived from 'sitekick/hosting/platform' and 'cardinality.to'
    cardinality:
      from: inf  # Can be inf(inite), a fixed number (1, 2...) or an expression
      to: 1
  sitekick/hosting/server:
    name:
      from: domains  # Can be derived from 'sitekick/hosting/domain' and 'cardinality.from'
      to: server  # Can be derived from 'sitekick/hosting/server' and 'cardinality.to'
    cardinality:
      from: inf  # Can be inf(inite), a fixed number (1, 2...) or an expression
      to: 1
```
