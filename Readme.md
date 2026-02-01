# BENCHMARKING TEST SUITE

This repository contains three independent benchmark runners for: -
Redis (hiredis) - Memcached (libmemcached) - Graph Databases (Neo4j +
SNAP)

  --------------
  REQUIREMENTS
  --------------

COMMON (all benchmarks) - bash - make - gcc or clang - awk, sed - curl

REDIS (hiredis) - redis-server - redis-cli - hiredis (built locally in
benchmark directory)

MEMCACHED (libmemcached) - memcached - libmemcached (or bundled sources)

GRAPH DATABASE (Neo4j + SNAP) - Neo4j server - Java (OpenJDK) -
libcurl + curl-config - SNAP + GLib (built via make all)

  ------------------------------
  BENCHMARK 1: REDIS (HIREDIS)
  ------------------------------

Location: benchlmarking_test/hiredisben/hiredis/

Run: ./run_bench_hiredis.sh

Description: - Starts Redis on port 6379 - Compiles hiredis benchmarks
(h1b, h2b, h3b, h4b) - Runs workloads on DB01.csv .. DB09.csv -
Aggregates latency results

Output: \_tmp/\_JHFVB0.csv

  ---------------------------------------
  BENCHMARK 2: MEMCACHED (LIBMEMCACHED)
  ---------------------------------------

Location: benchlmarking_test/memcben/libmemcached/

Run: ./run_bench_memcached.sh

Description: - Loads datasets using load_1B.sh - Aggregates multiple CSV
runs - Groups results (0--9, 10--19, 20--29, 30--39) - Runs
post-processing via load_2C.sh

Output: \_temp_res/\_tmp_res.txt \_temp_res/*tmp*\*.csv

  --------------------------------------------
  BENCHMARK 3: GRAPH DATABASE (NEO4J + SNAP)
  --------------------------------------------

Location: benchlmarking_test/memgraph/test/

Run: ./run_bench_graphdb.sh

Description: - Builds SNAP benchmarks (R2_benchmark, R4_benchmark) -
Builds Neo4j loader (test-flowneo4j) - Restarts Neo4j and waits for HTTP
readiness - Runs Neo4j loads on: datasets/10mb datasets/20mb
datasets/40mb datasets/80mb - Runs SNAP BFS and LWCC benchmarks

Output: \_temp/results/\_tmp_res\*.csv (SNAP benchmarks)
\_temp/results/\_temp_res*c*.csv (Neo4j latency results)

  ----------
  DATASETS
  ----------

Redis: benchlmarking_test/hiredisben/hiredis/dataset/ DB01.csv ..
DB09.csv

Graph DB: benchlmarking_test/memgraph/test/datasets/ 10mb/part1.txt
20mb/part2.txt 40mb/part3.txt 80mb/part4.txt

  -------------
  QUICK START
  -------------

Redis: cd benchlmarking_test/hiredisben/hiredis ./run_bench_hiredis.sh

Memcached: cd ../../memcben/libmemcached ./run_bench_memcached.sh

Graph DB: cd ../../memgraph/test ./run_bench_graphdb.sh

  -----------------
  TROUBLESHOOTING
  -----------------

Neo4j not reachable: lsof -nP -iTCP:7474 -sTCP:LISTEN brew services list
\| grep neo4j curl http://localhost:7474

Redis not responding: redis-cli -p 6379 ping

Permission errors: chmod +x \*.sh

  -------------
  END OF FILE
  -------------
