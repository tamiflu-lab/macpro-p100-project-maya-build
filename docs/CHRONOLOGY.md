# 作業時系列と「先行」の意味 / Timeline and scope of the claim

## 日本語

このリポジトリの主題は、**2026年10月16日に公開されたProject Maya v1.1.14のPascal実験対応を利用する前に、2026年10月15日に独自修正を行い、Tesla P100を含む実機で推論を成立させた作業記録**です。

| 日付 | 出来事 | 確認方法・証拠の位置付け |
|---|---|---|
| **2026-10-14** | Project Maya **v1.1.9**リリース | [GitHub公開リリース記録](https://github.com/mw00/project-maya/releases/tag/v1.1.9)、ベースのコミット `4c9f1b1` |
| **2026-10-15** | v1.1.9の作業ツリーでCUDA `sm_60` を含む独自互換性修正、P100×4＋GP100×1の起動と推論 | 当時のビルド・初回起動ログ、実機の変更ファイル。ログ全文は未公開 |
| **2026-10-16** | Project Maya **v1.1.14**リリース。Pascal Compute Capability 6.xの実験的対応が公表される | [GitHub公開リリース記録](https://github.com/mw00/project-maya/releases/tag/v1.1.14) |
| **2026-10-17** | 実機構築の日本語記事を公開 | [note構築記録](https://note.com/tamiflu_lab/n/ndbda115333f9) |

### 主張すること

- **10月15日の実機検証は、公式v1.1.14の公開日（10月16日）より前だった。**
- 当時のベースは**Project Maya v1.1.9＋未コミットのローカル修正**だった。
- `sm_60`、PCIe認識、NextN/MTP、複数GPUとRAMの分担などの問題を実機で切り分けた。

### 主張しないこと

- 「世界で初めてP100対応を実装した」「公式の開発者より先に開発した」。
- 上流のPull Request、非公開ブランチ、他のユーザーの検証より先だった。
- 上流`v1.1.14`の未改造コードで同じ速度を測った。
- 独自修正が、公式修正と異なる／優れている／今も必要である。

公式`v1.1.14`ではTesla P100への実験的なビルド対象拡張が告知されていますが、当時の実装差分と現在の公式版でのP100動作は別途検証が必要です。

## English

This repository documents **my October 15, 2026 bring-up of a locally modified Project Maya v1.1.9 tree on four Tesla P100s and one Quadro GP100**, before the **public release of v1.1.14 on October 16** which announced experimental Pascal 6.x support. The claim concerns **public release chronology only**, not upstream development priority or a world-first implementation. Other contributors may already have been working on Pascal support. The current machine has not been benchmarked on an unmodified v1.1.14 tree.

Sources: [v1.1.9](https://github.com/mw00/project-maya/releases/tag/v1.1.9), [v1.1.14](https://github.com/mw00/project-maya/releases/tag/v1.1.14), [note](https://note.com/tamiflu_lab/n/ndbda115333f9).
