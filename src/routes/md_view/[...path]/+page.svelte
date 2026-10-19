<script lang="ts">
  import { page } from '$app/state';
  import { joinURL } from 'ufo';
  import { marked } from 'marked';
  import fm from 'front-matter';

  import PreviewLayout from '#lib/components/previewLayout.svelte';

  // UI状態の定義
  let htmlContent = $state('');
  let loading = $state(true);
  let errorMsg = $state('');

  // [...path]の変更を監視して，Markdownを取得し直す
  $effect(() => {
    const currentPath = page.params.path;

    loadMarkdown(currentPath || '');
  });

  async function loadMarkdown(pathParam: string) {
    htmlContent = '';
    loading = true;
    errorMsg = '';

    try {
      const path = pathParam;
      const mdUrl = joinURL('/', path);

      const res = await fetch(mdUrl);

      if (!res.ok) {
        if (res.status === 404) {
          throw new Error('該当するファイルが見つかりませんでした');
        }
        throw new Error('読み込みに失敗しました');
      }

      const text = await res.text();
      const parsed = fm(text);
      htmlContent = await marked.parse(parsed.body);
    } catch (err) {
      errorMsg = (err as Error).message;
    } finally {
      loading = false;
    }
  }
</script>

<svelte:head>
  <title>{page.params.path} - プレビュー</title>
</svelte:head>

<PreviewLayout fileName={page.params.path || ''}>
  {#if loading}
    <p>読み込み中...</p>
  {:else if errorMsg}
    <p class="error">{errorMsg}</p>
  {:else}
    <article class="markdown-body">
      {@html htmlContent}
    </article>
  {/if}
</PreviewLayout>
