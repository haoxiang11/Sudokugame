@media (max-width: 768px) {
  body {
    padding: 5px;
    justify-content: flex-start; /* 顶部开始布局，空间可能不够 */
  }

  .game-screen {
    padding: 10px;
  }

  .cell {
    font-size: 1rem; /* 调小字体 */
    min-height: 40px; /* 保证最小触控区域 */
  }

  .cell-number {
    font-size: 1rem;
  }

  .number-pad {
    grid-gap: 5px;
  }

  .number-btn, .action-btn {
    font-size: 1rem;
    min-height: 44px; /* 触控友好 */
  }

  .game-controls {
    grid-template-columns: 1fr; /* 单列布局，按钮上下排列 */
    grid-gap: 8px;
  }

  .control-btn {
    padding: 15px; /* 增大按钮 */
  }

  /* 调整标题和状态信息字体大小 */
  .game-header h1 {
    font-size: 1.5rem;
  }
  .game-info, .game-stats {
    font-size: 0.85rem;
  }
}
