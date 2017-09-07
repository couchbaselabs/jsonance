jsonance - WIP / library for analyzing JSON for metadata

jsonance, rhymes with "resonance", is a ... [todo]

    j := new(options)
    j := open(previousAnalysis, options)

    r := j.reader()

    r2 := r.prevReader() // Navigate to past

    b := j.createBranch(branchName, options)

    b2 := b.fork(options)

    b.close()

    b.summary()

    batch.setOpaque(opaqueKey, opaqueVal)

    a := j.analyze(doc)

    batch.add(vbucketId, seq, key, a)
    batch.delete(vbucketId, seq, key)
    batch.commit()
    batch.close()

    j.analysis()

from cbdatasource (or any data source)...

    get-opaque
      j.reader.getOpaque()

    rollbackEx(vbucketID uint16, vbucketUUID uint64, rollbackSeq uint64) error

    onSnapshotStart(vbucketID uint16, snapStartSeq, snapEndSeq uint64, snapType uint32)

    set-opaque(vbucketID uint64, []byte)
      b.setOpaque

    get-opaque(vbucketID uint64) ([]byte, lastSeq, err)
      b.getOpaque

    DataUpdate(vbucketID uint16, key []byte, seq uint64, r *gomemcached.MCRequest)
      b.onMutation

    DataDelete(vbucketID uint16, key []byte, seq uint64, r *gomemcached.MCRequest)

analysis thoughts

    want the first time a field shows up
    want the first time a "fingerprint" (multi-field schema) shows up
    want the last time a field shows up
    want the last time a fingerprint shows up

    what does "first time" / "last time" mean?

    do missing fields mean it's a different fingerprint?
    are brand new additional field(s) detected like inheritance relationship?  "contains-a" / has-a?

example analysis

    source: {
      sourceName: "..."
    },
    branches: {
      "": {
      },
      "20180829-234123": {
        parent: ""
        opaque: {
        }
      }
    }

example PINDEX_META...

    {
      "name": "bs0_5ea163404f446bb6_13aa53f3",
      "uuid": "ad2b4749569cafe4",
      "indexType": "fulltext-index",
      "indexName": "bs0",
      "indexUUID": "5ea163404f446bb6",
      "indexParams": "{\"doc_config\":{\"mode\":\"type_field\",\"type_field\":\"type\"},\"mapping\":{\"default_analyzer\":\"standard\",\"default_datetime_parser\":\"dateTimeOptional\",\"default_field\":\"_all\",\"default_mapping\":{\"dynamic\":false,\"enabled\":true,\"properties\":{\"description\":{\"dynamic\":false,\"enabled\":true,\"fields\":[{\"analyzer\":\"\",\"include_in_all\":false,\"include_term_vectors\":false,\"index\":true,\"name\":\"description\",\"store\":false,\"type\":\"text\"}]}}},\"default_type\":\"_default\",\"index_dynamic\":false,\"store_dynamic\":false},\"store\":{\"kvStoreName\":\"mossStore\"}}",
      "sourceType": "couchbase",
      "sourceName": "beer-sample",
      "sourceUUID": "8f6e4f2e74d953213609fdd59396f6a9",
      "sourceParams": "{}",
      "sourcePartitions": "0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,118,119,120,121,122,123,124,125,126,127,128,129,130,131,132,133,134,135,136,137,138,139,140,141,142,143,144,145,146,147,148,149,150,151,152,153,154,155,156,157,158,159,160,161,162,163,164,165,166,167,168,169,170"
    }

  sourcePoint: "2934"
  sourcePoints: {
    "2934": {
      parent: "2933"
    }
  }

verbiage / trying to name the historic data points...
  srcRev
  snapshots
  points
  versions
  sha (a'la git sha)
  token
  savepoints
  rollback
  ref
  id
  tag
  generation / genTag
  ancestry / ancestor tag
  birth record
  fingerprint
  lineage point
  pedigree
  descent
  source context

population
populace
colony
settlers

// ParseFailOverLog parses a byte array to an array of [vbucketUUID,
// seqNum] pairs.
func ParseFailOverLog(body []byte) ([][]uint64, error) {
	flog := make([][]uint64, len(body)/16)
	for i, j := 0, 0; i < len(body); i += 16 {
		uuid := binary.BigEndian.Uint64(body[i : i+8])
		seqn := binary.BigEndian.Uint64(body[i+8 : i+16])
		flog[j] = []uint64{uuid, seqn}
		j++
	}
	return flog, nil
}


failOverLog...
  vbID => vbUUID => seqNum

