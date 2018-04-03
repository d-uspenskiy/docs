---
title: 5. Auto Rebalancing
linkTitle: 5. Auto Rebalancing
description: Auto Rebalancing
aliases:
  - /explore/auto-rebalancing/
menu:
  latest:
    identifier: auto-rebalancing
    parent: explore
    weight: 250
---

YugaByte DB automatically rebalances data into newly added nodes, so that the cluster can easily be expanded if more space is needed. In this tutorial, we will look at how YugaByte rebalances data while a workload is running. We will run a read-write workload using a pre-packaged sample application against a 3-node local universe with a replication factor of 3, and add nodes to it while the workload is running. We will then observe how the cluster rebalances its on-disk data as well as its memory footprint.

If you haven't installed YugaByte DB yet, do so first by following the [Quick Start](../../quick-start/install/) guide.

<ul class="nav nav-tabs nav-tabs-yb">
  <li class="active">
    <a href="#docker">
      <i class="icon-docker"></i>
      Docker
    </a>
  </li>
  <li >
    <a href="#macos">
      <i class="fa fa-apple" aria-hidden="true"></i>
      macOS
    </a>
  </li>
  <li>
    <a href="#linux">
      <i class="fa fa-linux" aria-hidden="true"></i>
      Linux
    </a>
  </li>
</ul>

<div class="tab-content">
  <div id="docker" class="tab-pane fade in active">
    {{% includeMarkdown "docker/auto-rebalancing.md" /%}}
  </div>
  <div id="kubernetes" class="tab-pane fade">
    {{% includeMarkdown "kubernetes/auto-rebalancing.md" /%}}
  </div>
  <div id="macos" class="tab-pane fade">
    {{% includeMarkdown "binary/auto-rebalancing.md" /%}}
  </div>
  <div id="linux" class="tab-pane fade">
    {{% includeMarkdown "binary/auto-rebalancing.md" /%}}
  </div> 
</div>