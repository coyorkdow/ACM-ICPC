```c++
template <typename T, class compare = less<T>>
class sparse_table {
 private:
  vector<vector<T>> st;
  compare rmq;
  inline T __rmq(const T &l, const T &r) const { return rmq(l, r) ? l : r; }

 public:
  int n;
  sparse_table(int _n, const vector<T> &v) : n(_n) {
    int k = (int)log2(n) + 1;
    st.assign(n, vector<T>(k));
    for (int i = 0; i < n; i++) st[i][0] = v[i];
    for (int i = 1; i < k; i++) {
      for (int j = 0; j + (1 << i) - 1 < n; j++)
        st[j][i] = __rmq(st[j][i - 1], st[j + (1 << (i - 1))][i - 1]);
    }
  }
  inline T query(int l, int r) {
    int k = (int)log2(r - l);
    return __rmq(st[l][k], st[r - (1 << k)][k]);
  }
};
```

