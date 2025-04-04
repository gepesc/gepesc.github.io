---
layout: default
title: Projetos
permalink: /projetos/
---

<h1>Projetos Públicos</h1>
<div id="projetos"></div>

<script>
  const username = "gepesc";
  const reposExcluidos = ["https://github.com/gepesc/gepesc.github.io"]; // Repositório da página

  async function fetchRepos() {
    const response = await fetch(`https://api.github.com/users/${username}/repos`);
    const data = await response.json();

    const container = document.getElementById("projetos");
    for (const repo of data) {
      if (reposExcluidos.includes(repo.name)) continue;

      const readmeRes = await fetch(`https://api.github.com/repos/${username}/${repo.name}/readme`, {
        headers: { Accept: "application/vnd.github.v3.html" }
      });

      const readmeHTML = await readmeRes.text();

      const card = document.createElement("div");
      card.style.border = "1px solid #ccc";
      card.style.padding = "10px";
      card.style.marginBottom = "20px";
      card.innerHTML = `
        <h2><a href="${repo.html_url}" target="_blank">${repo.name}</a></h2>
        <div>${readmeHTML}</div>
      `;
      container.appendChild(card);
    }
  }

  fetchRepos();
</script>
