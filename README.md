# Ansibleのテストユース

## ドキュメントなど

- [公式ドキュメント](https://docs.ansible.com/)
- [Ansibleのローカル実行](https://qiita.com/hiroyuki_onodera/items/e6d0d308eb44e26fa03f#playbook-1)
  - test.ymlはここからコピペした。

## 疑問

- インベントリーってなに？
- playbookってこれは定義ファイルになる？
- sudoどうしようね。どのユーザーでログインするのがbest practice?どこに書く？

### ユーザーの書き方

Hostと一緒のところにかける。

### ベストプラクティス

https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#where-does-the-configuration-file-live-and-what-can-i-configure-in-it

見てねとある。

### ドキュメントのエントリーポイント

readthedocの最初から追うより、[Traditional Table of Contents](https://docs.ansible.com/ansible/latest/user_guide/index.html#traditional-table-of-contents)から見るのがいい。ところでAnsibleのドキュメント読みやすい。なんで読みやすいんだろう。

### sudoどうしよう

[become](https://docs.ansible.com/ansible/latest/user_guide/become.html)が詳しい。

ansibleでは定義ファイル内で、become句？が用意されていて、yesと入れるとprivilege userに昇格する。公式ページでは、

```yaml
- name: Ensure the httpd service is running
  service:
    name: httpd
    state: started
  become: yes
```

と示されている。これでrootに昇格してsystemctlでstartedになるように実行してくれるらしい。(あくまでstarted, startではない。startは動作だけどstartedってstateだよね。状態記述の気持ちが現れている)

実行時`ansible-playbook --ask-become-pass` (`-K`が同じ)とすることで、sudoのpasswordを指定することができる。

### インベントリーってなに？

[basic concept](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html)に用語集があって、ここに

> A list of managed nodes

とある。`hostfile`とも呼ばれるとあって納得。

### なんでAnsibleのドキュメント読みやすいんだろう。

僕はまず概念的なことはある程度知っていたので、どうやって使うんだろうな〜ということを念頭に置いていた。

- ユースケースがはっきりしていた
  - Kubuntuを研究室と家の両方で使う。両方に同じ操作を行うのはめんどい。
  - 研究室のクラスター管理にも使えそうだと期待していた。
- ドキュメントのエントリーポイントをミスらなかった。
  - basic_concepts, traditional...をはじめに見られたのはでかい。
  - ウェブサイトを作成するときはいつも注意したほうがよさそう。
  - Google検索もエントリーポイントなので、ブログとかじゃない限り、検索にヒットしないページの設定も大事かもしれない。
- 各ページで、知りたいことがちゃんと余すことなく乗ってる。
- 読みやすい英語
  - 結局これ
  - 文法が正しくて、簡単な単語を使ってくれている
  - "documentation is the best abstaraction"(betterだったかも)
