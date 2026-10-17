<script lang="ts">
  import { onMount } from 'svelte';

  type Language = 'pt' | 'en';
  let lang = $state<Language>('pt');
  let mobileOpen = $state(false);
  let activeImage = $state<number | null>(null);
  let lightboxElement = $state<HTMLDialogElement>();
  let sent = $state(false);
  let formError = $state(false);

  const whatsapp = 'https://wa.me/5583998739712';
  const images = [
    { src: '/images/hero-casa-candeia.webp', width: 1536, height: 1024, alt: { pt: 'Uma hospedaria conceitual entre montanhas cobertas pela névoa da manhã', en: 'A conceptual mountain inn among ridges veiled in morning mist' }, label: { pt: 'A serra desperta devagar', en: 'The mountains wake slowly' } },
    { src: '/images/acomodacao-refugio.webp', width: 1122, height: 1402, alt: { pt: 'Quarto ilustrativo com cama de linho e vista para as montanhas', en: 'Illustrative guest room with linen bedding and a mountain view' }, label: { pt: 'Um quarto, muitas pausas', en: 'A room for unhurried moments' } },
    { src: '/images/experiencia-trilha.webp', width: 1586, height: 992, alt: { pt: 'Trilha ilustrativa entre vegetação de montanha e horizonte enevoado', en: 'Illustrative mountain trail through greenery and a misty horizon' }, label: { pt: 'Caminhos sem pressa', en: 'Paths at your own pace' } },
    { src: '/images/galeria-cafe.webp', width: 1448, height: 1086, alt: { pt: 'Mesa de café da manhã ilustrativa junto a uma janela para a serra', en: 'Illustrative breakfast table beside a window overlooking the mountains' }, label: { pt: 'O tempo de uma manhã', en: 'The shape of a morning' } },
    { src: '/images/serra-nevoa-manha.webp', width: 1536, height: 1024, alt: { pt: 'Camadas de montanhas e mata atlântica sob a luz suave da manhã', en: 'Mountain and Atlantic Forest layers in soft morning light' }, label: { pt: 'Luz atravessando a névoa', en: 'Light moving through the mist' } },
    { src: '/images/recanto-janela.webp', width: 1122, height: 1402, alt: { pt: 'Poltrona de linho junto a uma janela com vista para a serra, em imagem conceitual', en: 'Linen armchair by a window overlooking the mountains in a concept image' }, label: { pt: 'Um canto junto à janela', en: 'A quiet corner by the window' } },
    { src: '/images/caminho-da-serra.webp', width: 1536, height: 1024, alt: { pt: 'Caminho estreito entre vegetação de montanha com a serra ao fundo', en: 'A narrow path through mountain greenery with ridges beyond' }, label: { pt: 'Por onde a trilha continua', en: 'Where the path continues' } }
  ];

  const mapEmbed = 'https://www.google.com/maps?q=Parque+Capivari%2C+Diogo+Jos%C3%A9+de+Carvalho%2C+1291%2C+Capivari%2C+Campos+do+Jord%C3%A3o+-+SP&output=embed';
  const mapLink = 'https://www.google.com/maps/search/?api=1&query=Parque+Capivari%2C+Diogo+Jos%C3%A9+de+Carvalho%2C+1291%2C+Campos+do+Jord%C3%A3o%2C+SP';

  const copy = {
    pt: {
      nav: ['Acomodações', 'Experiências', 'Galeria', 'Como chegar', 'Dúvidas'],
      eyebrow: 'HOSPEDARIA DE MONTANHA · CONCEITO DE PORTFÓLIO',
      titleFirst: 'A serra no seu', titleSecond: 'próprio ritmo.',
      lead: 'Um convite imaginado para desacelerar, abrir a janela e deixar o dia acontecer sem pressa.',
      cta: 'Consultar uma estadia',
      scroll: 'UM LUGAR PARA RESPIRAR',
      introLabel: '01 / A CASA',
      introTitle: 'Entre o silêncio da serra e o calor de chegar.',
      intro: 'Casa Candeia é um estudo de marca e hospedaria fictícia. Imaginamos uma casa pequena, cercada por verde, onde cada detalhe ajuda a encontrar um ritmo mais leve.',
      accommodationsLabel: '02 / ACOMODAÇÕES',
      accommodationsTitle: 'Espaços para descansar de verdade.',
      roomTitle: 'Refúgio de dois',
      roomText: 'Uma composição de quarto com materiais naturais, luz suave e a paisagem sempre por perto. Imagem e capacidade são ilustrativas; os itens reais seriam definidos com a operação.',
      roomTag: 'ESTUDO DE ACOMODAÇÃO',
      roomLink: 'Perguntar sobre acomodações',
      experiencesLabel: '03 / EXPERIÊNCIAS',
      experiencesTitle: 'A melhor programação pode ser não ter nenhuma.',
      experiencesText: 'Caminhar, observar a mudança da luz ou simplesmente ficar. As experiências aqui são uma direção de conteúdo; atividades disponíveis dependeriam da localização e da operação reais.',
      experienceTag: 'SERRA · TEMPO · PRESENÇA',
      galleryLabel: '04 / CADERNO VISUAL',
      galleryTitle: 'Pequenas cenas de uma estadia imaginada.',
      galleryText: 'Imagens conceituais criadas para apresentar a direção visual da Casa Candeia.',
      galleryImageNote: 'CADERNO DE ESTADIA · IMAGEM CONCEITUAL',
      openImage: 'Ampliar imagem',
      locationLabel: '05 / COMO CHEGAR',
      locationTitle: 'Uma referência no coração da serra.',
      locationText: 'O mapa aponta para o Parque Capivari, em Campos do Jordão (SP), como referência pública na Serra da Mantiqueira. A Casa Candeia é fictícia e não funciona neste endereço.',
      locationMeta: 'PONTO DE REFERÊNCIA REAL · POUSADA FICTÍCIA',
      mapLabel: 'Mapa do Parque Capivari, em Campos do Jordão, São Paulo',
      mapAddressLabel: 'REFERÊNCIA GEOGRÁFICA',
      mapAddress: 'Parque Capivari · Diogo José de Carvalho, 1291 · Capivari · Campos do Jordão — SP',
      mapDisclaimer: 'Este é um ponto público real para contextualizar o conceito. Não é o endereço de uma hospedaria em funcionamento.',
      mapLinkLabel: 'Abrir referência no Google Maps',
      faqLabel: '06 / ANTES DE ESCOLHER AS DATAS',
      faqTitle: 'Perguntas que merecem uma resposta clara.',
      faqs: [
        ['A Casa Candeia está recebendo hóspedes?', 'Não. Esta é uma marca fictícia criada para portfólio. O formulário demonstra um fluxo de consulta e não confirma uma reserva real.'],
        ['Onde fica a hospedaria?', 'A pousada é fictícia. O mapa mostra o Parque Capivari, em Campos do Jordão, apenas como referência geográfica real na Serra da Mantiqueira.'],
        ['Quais são as diárias e a disponibilidade?', 'Valores, datas disponíveis e condições de hospedagem não foram definidos. A página não faz reserva nem confirma disponibilidade.'],
        ['As imagens mostram os espaços reais?', 'Não. São imagens conceituais geradas para este projeto de portfólio.']
      ],
      bookingLabel: '07 / CONSULTA DE ESTADIA',
      bookingTitle: 'Conte como seria a sua pausa.',
      bookingText: 'Envie um pedido conceitual de consulta. Em uma operação real, a equipe confirmaria disponibilidade, valores e detalhes antes de qualquer reserva.',
      formName: 'Seu nome', formEmail: 'E-mail', formPhone: 'WhatsApp ou telefone', formArrival: 'Data de chegada desejada', formDeparture: 'Data de saída desejada', formGuests: 'Número de hóspedes', formGuestsOptions: ['Escolha uma opção', '1 hóspede', '2 hóspedes', '3 hóspedes', '4 ou mais'], formMessage: 'O que você gostaria de saber?', formConsent: 'Entendo que esta é uma demonstração conceitual e não uma reserva real.', formSubmit: 'Enviar consulta conceitual', formRequired: 'Obrigatório', formSuccess: 'Consulta demonstrativa enviada. Em uma hospedaria real, a equipe retornaria para confirmar os detalhes.', formError: 'Não foi possível enviar agora. Tente novamente ou use o WhatsApp.', formPrivacy: 'Os dados deste formulário são destinados apenas à demonstração do fluxo. Política de privacidade final precisa ser definida antes do uso real.',
      confirmationTitle: 'Seu pedido foi registrado.',
      whatsapp: 'Prefere conversar pelo WhatsApp?', whatsappCta: 'Abrir conversa',
      footerLine: 'UM CONCEITO DE HOSPEDARIA PARA RESPIRAR MAIS DEVAGAR.', footerConcept: 'Marca e imagens conceituais. O mapa indica uma referência pública; hospedagem, serviços e disponibilidade não definidos.',
      languageLabel: 'Mudar idioma para inglês'
    },
    en: {
      nav: ['Rooms', 'Experiences', 'Gallery', 'Getting here', 'Questions'],
      eyebrow: 'MOUNTAIN INN · PORTFOLIO CONCEPT',
      titleFirst: 'The mountains,', titleSecond: 'at your own pace.',
      lead: 'An imagined invitation to slow down, open the window and let the day unfold without hurry.',
      cta: 'Ask about a stay',
      scroll: 'A PLACE TO BREATHE',
      introLabel: '01 / THE HOUSE',
      introTitle: 'Between the quiet of the mountains and the warmth of arriving.',
      intro: 'Casa Candeia is a fictional hospitality brand concept. We imagine a small house surrounded by green, where every detail helps you find a lighter rhythm.',
      accommodationsLabel: '02 / ROOMS',
      accommodationsTitle: 'Spaces made for real rest.',
      roomTitle: 'A retreat for two',
      roomText: 'A room concept with natural materials, soft light and the landscape close by. The image and capacity are illustrative; real amenities would be defined with the operation.',
      roomTag: 'ROOM CONCEPT',
      roomLink: 'Ask about the rooms',
      experiencesLabel: '03 / EXPERIENCES',
      experiencesTitle: 'The best itinerary may be having none at all.',
      experiencesText: 'Walk, watch the light shift, or simply stay in. Experiences here are a content direction; actual activities would depend on a real location and operation.',
      experienceTag: 'MOUNTAINS · TIME · PRESENCE',
      galleryLabel: '04 / VISUAL JOURNAL',
      galleryTitle: 'Small scenes from an imagined stay.',
      galleryText: 'Concept images created to introduce the Casa Candeia visual direction.',
      galleryImageNote: 'STAY JOURNAL · CONCEPT IMAGE',
      openImage: 'Enlarge image',
      locationLabel: '05 / GETTING HERE',
      locationTitle: 'A real point in the mountains.',
      locationText: 'The map points to Parque Capivari in Campos do Jordão, São Paulo, as a public landmark in the Serra da Mantiqueira. Casa Candeia is fictional and does not operate at this address.',
      locationMeta: 'REAL LANDMARK · FICTIONAL INN',
      mapLabel: 'Map of Parque Capivari in Campos do Jordão, São Paulo',
      mapAddressLabel: 'GEOGRAPHIC REFERENCE',
      mapAddress: 'Parque Capivari · Diogo José de Carvalho, 1291 · Capivari · Campos do Jordão — SP',
      mapDisclaimer: 'This is a real public landmark used to contextualize the concept. It is not the address of an operating inn.',
      mapLinkLabel: 'Open reference in Google Maps',
      faqLabel: '06 / BEFORE CHOOSING YOUR DATES',
      faqTitle: 'Questions deserve clear answers.',
      faqs: [
        ['Is Casa Candeia welcoming guests?', 'No. This is a fictional portfolio concept. The form demonstrates an inquiry flow and does not confirm a real booking.'],
        ['Where is the inn?', 'The inn is fictional. The map shows Parque Capivari in Campos do Jordão only as a real geographic reference in the Serra da Mantiqueira.'],
        ['What are the rates and availability?', 'Rates, available dates and stay conditions have not been defined. This page does not book or confirm availability.'],
        ['Do the images show the real rooms?', 'No. These are conceptual images generated for this portfolio project.']
      ],
      bookingLabel: '07 / STAY INQUIRY',
      bookingTitle: 'Tell us what your pause might look like.',
      bookingText: 'Send a conceptual inquiry. In a real operation, the team would confirm availability, rates and details before any booking.',
      formName: 'Your name', formEmail: 'Email', formPhone: 'WhatsApp or phone', formArrival: 'Desired arrival date', formDeparture: 'Desired departure date', formGuests: 'Number of guests', formGuestsOptions: ['Choose an option', '1 guest', '2 guests', '3 guests', '4 or more'], formMessage: 'What would you like to know?', formConsent: 'I understand this is a conceptual demo and not a real reservation.', formSubmit: 'Send conceptual inquiry', formRequired: 'Required', formSuccess: 'Demo inquiry submitted. A real inn team would follow up to confirm the details.', formError: 'We could not send this right now. Please try again or use WhatsApp.', formPrivacy: 'Form data is intended only to demonstrate the flow. A final privacy policy must be defined before real use.',
      confirmationTitle: 'Your request was recorded.',
      whatsapp: 'Would you rather chat on WhatsApp?', whatsappCta: 'Open conversation',
      footerLine: 'A LODGING CONCEPT FOR A SLOWER BREATH.', footerConcept: 'Concept brand and imagery. The map marks a public landmark; lodging, services and availability are undefined.',
      languageLabel: 'Change language to Portuguese'
    }
  };
  let t = $derived(copy[lang]);

  $effect(() => {
    if (activeImage !== null) queueMicrotask(() => { if (lightboxElement && !lightboxElement.open) lightboxElement.showModal(); });
  });

  function toggleLanguage() {
    lang = lang === 'pt' ? 'en' : 'pt';
    document.documentElement.lang = lang === 'pt' ? 'pt-BR' : 'en';
    localStorage.setItem('casa-candeia-lang', lang);
  }

  function submitDemo(event: SubmitEvent) {
    event.preventDefault();
    const form = event.currentTarget as HTMLFormElement;
    const data = new FormData(form);
    const body = new URLSearchParams();
    data.forEach((value, key) => body.append(key, String(value)));
    formError = false;
    fetch('/', { method: 'POST', headers: { 'Content-Type': 'application/x-www-form-urlencoded' }, body: body.toString() })
      .then((response) => { if (response.ok) sent = true; else formError = true; })
      .catch(() => { formError = true; });
  }

  function closeLightbox() {
    if (lightboxElement?.open) lightboxElement.close();
    activeImage = null;
  }

  onMount(() => {
    const saved = localStorage.getItem('casa-candeia-lang');
    if (saved === 'en') { lang = 'en'; document.documentElement.lang = 'en'; }
  });
</script>

<svelte:head>
  <title>{lang === 'pt' ? 'Casa Candeia — A serra no seu próprio ritmo' : 'Casa Candeia — The mountains, at your own pace'}</title>
  <meta name="description" content={lang === 'pt' ? 'Casa Candeia: hospedaria de montanha fictícia para portfólio, com imagens conceituais e mapa de referência real em Campos do Jordão.' : 'Casa Candeia: a fictional mountain inn portfolio concept with conceptual imagery and a real map reference in Campos do Jordão.'} />
  <meta property="og:title" content={lang === 'pt' ? 'Casa Candeia — A serra no seu próprio ritmo' : 'Casa Candeia — The mountains, at your own pace'} />
  <meta property="og:description" content={lang === 'pt' ? 'Uma hospedaria de montanha imaginada para desacelerar.' : 'An imagined mountain inn for slowing down.'} />
  <meta property="og:image" content="/images/hero-casa-candeia.webp" />
  <meta property="og:type" content="website" />
</svelte:head>

<a class="skip-link" href="#conteudo">{lang === 'pt' ? 'Pular para o conteúdo' : 'Skip to content'}</a>

<header class="site-header">
  <a class="brand" href="#inicio" aria-label="Casa Candeia — início">
    <img src="/brand/casa-candeia-simbolo.svg" alt="" width="46" height="46" />
    <span>Casa Candeia</span>
  </a>
  <button class="menu-toggle" aria-expanded={mobileOpen} aria-controls="main-nav" onclick={() => mobileOpen = !mobileOpen} aria-label={mobileOpen ? (lang === 'pt' ? 'Fechar menu' : 'Close menu') : (lang === 'pt' ? 'Abrir menu' : 'Open menu')}>
    <span></span><span></span>
  </button>
  <nav id="main-nav" class:open={mobileOpen} aria-label={lang === 'pt' ? 'Navegação principal' : 'Main navigation'}>
    {#each t.nav as item, i}
      <a href={['#acomodacoes', '#experiencias', '#galeria', '#localizacao', '#duvidas'][i]} onclick={() => mobileOpen = false}>{item}</a>
    {/each}
    <a class="nav-cta" href="#consulta" onclick={() => mobileOpen = false}>{t.cta}</a>
  </nav>
  <button class="language-toggle" onclick={toggleLanguage} aria-label={t.languageLabel}>{lang === 'pt' ? 'EN' : 'PT'} <span aria-hidden="true">↗</span></button>
</header>

<main id="conteudo">
  <section class="hero" id="inicio" aria-labelledby="hero-title">
    <div class="hero-copy">
      <p class="eyebrow"><span class="eyebrow-dot"></span>{t.eyebrow}</p>
      <h1 id="hero-title"><span>{t.titleFirst}</span><em>{t.titleSecond}</em></h1>
      <p class="hero-lead">{t.lead}</p>
      <a class="button button-dark" href="#consulta">{t.cta}<span aria-hidden="true">↗</span></a>
      <p class="hero-note"><span class="line"></span>{t.scroll}</p>
    </div>
    <figure class="hero-visual">
      <picture>
        <source media="(max-width: 760px)" srcset="/images/hero-casa-candeia-mobile.webp" width="941" height="1672" />
        <img src="/images/hero-casa-candeia.webp" alt={images[0].alt[lang]} fetchpriority="high" width="1536" height="1024" />
      </picture>
      <div class="hero-stamp" aria-hidden="true"><span>CASA</span><span>CANDEIA</span><span class="stamp-sun">✳</span></div>
      <figcaption><span>01 / 07</span><span>{images[0].label[lang]}</span></figcaption>
    </figure>
    <div class="hero-index" aria-hidden="true">01<br /><span>—</span><br />07</div>
  </section>

  <section class="intro section-wrap" aria-labelledby="intro-title">
    <p class="section-kicker">{t.introLabel}</p>
    <div class="intro-main"><h2 id="intro-title">{t.introTitle}</h2><p>{t.intro}</p></div>
    <div class="contour-mark" aria-hidden="true"><img src="/brand/casa-candeia-simbolo.svg" alt="" /></div>
  </section>

  <section class="rooms section-wrap" id="acomodacoes" aria-labelledby="rooms-title">
    <div class="section-heading">
      <p class="section-kicker">{t.accommodationsLabel}</p><h2 id="rooms-title">{t.accommodationsTitle}</h2>
    </div>
    <div class="room-feature">
      <figure class="room-image framed-image">
        <img src="/images/acomodacao-refugio.webp" alt={images[1].alt[lang]} loading="lazy" width="1122" height="1402" />
        <span class="room-photo-code" aria-hidden="true">CASA CANDEIA · CADERNO 01</span>
        <figcaption><span>01</span><span>{t.roomTag}</span></figcaption>
      </figure>
      <article class="room-copy editorial-panel">
        <span class="panel-number">01</span>
        <p class="micro-label">{t.accommodationsLabel}</p>
        <h3>{t.roomTitle}</h3>
        <p>{t.roomText}</p>
        <a class="text-link" href="#consulta">{t.roomLink}<span aria-hidden="true">↗</span></a>
        <div class="panel-foot"><span>CASA CANDEIA</span><span>·</span><span>CONCEITO</span></div>
      </article>
    </div>
  </section>

  <section class="experience" id="experiencias" aria-labelledby="experience-title">
    <div class="experience-image"><img src="/images/experiencia-trilha.webp" alt={images[2].alt[lang]} loading="lazy" width="1586" height="992" /></div>
    <div class="experience-copy">
      <p class="section-kicker">{t.experiencesLabel}</p><h2 id="experience-title">{t.experiencesTitle}</h2><p>{t.experiencesText}</p>
      <span class="experience-tag">{t.experienceTag}</span>
    </div>
    <span class="experience-caption" aria-hidden="true">03 / 07 — {images[2].label[lang]}</span>
  </section>

  <section class="gallery section-wrap" id="galeria" aria-labelledby="gallery-title">
    <div class="gallery-heading"><div><p class="section-kicker">{t.galleryLabel}</p><h2 id="gallery-title">{t.galleryTitle}</h2></div><p>{t.galleryText}</p></div>
    <div class="gallery-grid">
      {#each images as img, i}
        <button class="gallery-item" onclick={() => activeImage = i} aria-label={`${t.openImage}: ${img.label[lang]}`}>
          <span class="gallery-visual"><img src={img.src} alt={img.alt[lang]} loading="lazy" width={img.width} height={img.height} /><span class="gallery-number">0{i + 1}</span><span class="gallery-open" aria-hidden="true">↗</span></span>
          <span class="gallery-caption"><span class="gallery-title">{img.label[lang]}<small>{t.galleryImageNote}</small></span></span>
        </button>
      {/each}
    </div>
  </section>

  <section class="location" id="localizacao" aria-labelledby="location-title">
    <div class="location-inner">
      <p class="section-kicker">{t.locationLabel}</p><h2 id="location-title">{t.locationTitle}</h2><p class="location-copy">{t.locationText}</p>
      <div class="location-address"><span class="location-meta">{t.mapAddressLabel}</span><address>{t.mapAddress}</address><p>{t.mapDisclaimer}</p><a href={mapLink} target="_blank" rel="noreferrer">{t.mapLinkLabel}<span aria-hidden="true">↗</span></a></div>
    </div>
    <div class="location-map"><iframe title={t.mapLabel} src={mapEmbed} loading="lazy" referrerpolicy="no-referrer-when-downgrade" allowfullscreen></iframe><span class="map-caption">CAMPOS DO JORDÃO · SERRA DA MANTIQUEIRA</span></div>
  </section>

  <section class="faq section-wrap" id="duvidas" aria-labelledby="faq-title">
    <div class="faq-heading"><p class="section-kicker">{t.faqLabel}</p><h2 id="faq-title">{t.faqTitle}</h2></div>
    <div class="faq-list">
      {#each t.faqs as faq, i}
        <details class="faq-item"><summary><span class="faq-index">0{i + 1}</span><span>{faq[0]}</span><span class="faq-plus" aria-hidden="true"></span></summary><p>{faq[1]}</p></details>
      {/each}
    </div>
  </section>

  <section class="booking" id="consulta" aria-labelledby="booking-title">
    <div class="booking-side">
      <p class="section-kicker">{t.bookingLabel}</p><h2 id="booking-title">{t.bookingTitle}</h2><p>{t.bookingText}</p>
      <div class="booking-contact"><span>{t.whatsapp}</span><a href={`${whatsapp}?text=${encodeURIComponent(lang === 'pt' ? 'Olá! Gostaria de consultar uma estadia na Casa Candeia.' : 'Hello! I would like to ask about a stay at Casa Candeia.')}`} target="_blank" rel="noreferrer">{t.whatsappCta}<span aria-hidden="true">↗</span></a></div>
    </div>
    <div class="booking-form-wrap">
      {#if sent}
        <div class="form-success" role="status"><img src="/brand/casa-candeia-simbolo.svg" alt="" /><h3>{t.confirmationTitle}</h3><p>{t.formSuccess}</p></div>
      {:else}
        {#if formError}<p class="form-error" role="alert">{t.formError}</p>{/if}
        <form name="consulta-estadia" method="POST" action="/?enviado=1#consulta" data-netlify="true" {...{'netlify-honeypot': 'bot-field'}} onsubmit={submitDemo}>
          <input type="hidden" name="form-name" value="consulta-estadia" />
          <p class="honeypot"><label>Do not fill this out: <input name="bot-field" /></label></p>
          <div class="form-row"><label>{t.formName}<span>*</span><input name="nome" type="text" autocomplete="name" required /></label><label>{t.formEmail}<span>*</span><input name="email" type="email" autocomplete="email" required /></label></div>
          <div class="form-row"><label>{t.formPhone}<input name="telefone" type="tel" autocomplete="tel" /></label><label>{t.formGuests}<span>*</span><select name="hospedes" required>{#each t.formGuestsOptions as option, i}<option value={i === 0 ? '' : String(i)} disabled={i === 0} selected={i === 0}>{option}</option>{/each}</select></label></div>
          <div class="form-row"><label>{t.formArrival}<input name="chegada" type="date" /></label><label>{t.formDeparture}<input name="saida" type="date" /></label></div>
          <label>{t.formMessage}<textarea name="mensagem" rows="3"></textarea></label>
          <label class="consent"><input type="checkbox" name="consentimento" required /><span>{t.formConsent}</span></label>
          <button class="button button-dark form-submit" type="submit">{t.formSubmit}<span aria-hidden="true">↗</span></button>
          <p class="privacy-note">{t.formPrivacy}</p>
        </form>
      {/if}
    </div>
  </section>
</main>

<footer class="site-footer">
  <a class="brand footer-brand" href="#inicio"><img src="/brand/casa-candeia-simbolo-claro.svg" alt="" width="42" height="42" /><span>Casa Candeia</span></a>
  <div class="footer-center"><p>{t.footerLine}</p><span>{t.footerConcept}</span></div>
  <div class="footer-credit"><span>© CASA CANDEIA · 2026</span><span>Desenvolvido por <strong>LMX Soluções Digitais/Segundo Almeida</strong></span></div>
</footer>

{#if activeImage !== null}
  <dialog bind:this={lightboxElement} class="lightbox" aria-label={lang === 'pt' ? 'Imagem ampliada' : 'Enlarged image'} onclose={() => activeImage = null} onclick={(event) => { if (event.target === event.currentTarget) closeLightbox(); }}>
    <button class="lightbox-close" autofocus onclick={closeLightbox} aria-label={lang === 'pt' ? 'Fechar imagem ampliada' : 'Close enlarged image'}>×</button>
    <figure><img src={images[activeImage].src} alt={images[activeImage].alt[lang]} /><figcaption>{images[activeImage].label[lang]}</figcaption></figure>
  </dialog>
{/if}
