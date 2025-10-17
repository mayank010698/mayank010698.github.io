---
title: "Sketching for Deep Distributed Learning"
authors: "<strong>Mayank Shrivastava</strong>, Berivan Isik, Qiaobo Li, Sanmi Koyejo, Arindam Banerjee"
venue: "NeuRIPS, 2024"
year: 2024
url: "https://proceedings.neurips.cc/paper_files/paper/2024/file/0c433d14179ee4d6598f0e3ed85d6bc2-Paper-Conference.pdf"
paper_link: "https://neurips.cc/virtual/2024/poster/96942"
tldr: "The high communication cost between the server and the clients is a significant bottleneck in scaling distributed learning for overparametrized deep models. One popular approach for reducing this communication overhead is randomized sketching. However, existing theoretical analyses for sketching-based distributed learning (sketch-DL) either incur a prohibitive dependence on the ambient dimension or need additional restrictive assumptions such as heavy-hitters. Nevertheless, despite existing pessimistic analyses, empirical evidence suggests that sketch-DL is competitive with its uncompressed counterpart, thus motivating a sharper analysis. In this work, we introduce a sharper ambient dimension-independent convergence analysis for sketch-DL using the second-order geometry specified by the loss Hessian. Our results imply ambient dimension-independent communication complexity for sketch-DL. We present empirical results both on the loss Hessian and overall accuracy of sketch-DL supporting our theoretical results. Taken together, our results provide theoretical justification for the observed empirical success of sketch-DL."
---
