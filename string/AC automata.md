```c++
class aho_corasick {
 public:
  vector<vector<int>> trie;
  vector<int> fail;
  vector<pii> word;
  aho_corasick() {
    trie.push_back(vector<int>(26));
    word.push_back(pii(0, 0));
  }
  void insert(const string &str, int w) {
    int u = 0;
    for (auto &i : str) {
      if (!trie[u][i - 'a']) {
        trie[u][i - 'a'] = trie.size();
        trie.push_back(vector<int>(26));
        word.push_back(pii(0, 0));
      }
      u = trie[u][i - 'a'];
    }
    if (word[u] == pii(0, 0))
      word[u] = {str.length(), w};
    else if (word[u].second > w)
      word[u] = {str.length(), w};
  }
  void build() {
    fail.resize(trie.size());
    queue<int> q;
    for (int i = 0; i < 26; i++)
      if (trie[0][i]) q.push(trie[0][i]);
    while (q.size()) {
      int u = q.front();
      q.pop();
      for (int i = 0; i < 26; i++) {
        if (trie[u][i])
          fail[trie[u][i]] = trie[fail[u]][i], q.push(trie[u][i]);
        else
          trie[u][i] = trie[fail[u]][i];
      }
    }
  }
};
```

