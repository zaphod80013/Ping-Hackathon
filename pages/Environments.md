- [[P1AS]]
- query-table:: true
  query-sort-by:: block
  query-sort-desc:: true
  #+BEGIN_QUERY
  {:title "All blocks with tag v1.16.0.0"
   :query [:find (pull ?b [*])
           :where
           [?p :block/name "v1.16.0.0"]
           [?b :block/refs ?p]]}
  #+END_QUERY
-