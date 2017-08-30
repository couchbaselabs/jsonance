jsonance - WIP / library for analyzing JSON for metadata

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




cbdatasource (or any data source)
  get-opaque
    j.reader.getOpaque()

  rollbackEx(vbucketID uint16, vbucketUUID uint64, rollbackSeq uint64) error

  onSnapshotStart

  set-opaque(vbucketID uint64, []byte)
    b.setOpaque

  get-opaque(vbucketID uint64) ([]byte, lastSeq, err)
    b.getOpaque

  DataUpdate(vbucketID uint16, key []byte, seq uint64, r *gomemcached.MCRequest)
    b.onMutation(

  DataDelete(vbucketID uint16, key []byte, seq uint64, r *gomemcached.MCRequest)




