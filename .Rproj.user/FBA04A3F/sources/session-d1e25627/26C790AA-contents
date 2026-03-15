# R/cv_helpers.R — no editar, edita _cv_data.yml
library(yaml)
library(htmltools)
library(here)

load_cv <- function(path = "_cv_data.yml") yaml::read_yaml(path)

# Extrae valor de un campo string o lista eu/en/es
tr <- function(field, lang) {
  if (is.null(field)) return("")
  if (is.character(field) && length(field) == 1) return(field)
  val <- field[[lang]]
  if (is.null(val)) return("")
  paste(val, collapse = " ")
}

# Convierte **bold** y [texto](url) a HTML
md_inline <- function(x) {
  x <- as.character(x)
  x <- gsub("\\*\\*(.+?)\\*\\*", "<strong>\\1</strong>", x, perl = TRUE)
  x <- gsub("\\[([^\\]]+)\\]\\(([^)]+)\\)",
             '<a href="\\2" target="_blank">\\1</a>', x, perl = TRUE)
  x
}
md_bold <- function(x) md_inline(x)

# Badge HTML
badge_html <- function(type, label) {
  if (is.null(type) || type == "" || is.null(label) || label == "") return("")
  sprintf('<span class="pub-badge %s">%s</span>', type, label)
}

# ── Secciones ──────────────────────────────────────────

build_profile <- function(cv, lang) {
  paras <- as.character(unlist(cv$profile[[lang]]))
  ps <- paste(sprintf("<p>%s</p>", sapply(paras, md_inline)), collapse = "\n")
  sprintf('<div class="bio-text">%s</div>', ps)
}

build_research_lines <- function(cv, lang) {
  lbls <- c(eu="Ikerketa ildoak", en="Lines of research", es="Líneas de investigación")
  rows <- sapply(cv$research_lines, function(r) {
    text <- tr(r, lang)
    if (grepl("^\\*\\*(.+?)\\*\\* ", text, perl = TRUE)) {
      label <- sub("^\\*\\*(.+?)\\*\\*.*", "<strong>\\1</strong>", text, perl = TRUE)
      desc  <- sub("^\\*\\*(.+?)\\*\\* ", "", text, perl = TRUE)
    } else {
      label <- md_inline(text)
      desc  <- ""
    }
    sprintf('<div class="rl-row"><div class="rl-label">%s</div><div class="rl-desc">%s</div></div>',
            label, desc)
  })
  sprintf('<div class="subsection-label">%s</div><div class="research-lines">%s</div>',
          lbls[[lang]], paste(rows, collapse = "\n"))
}

build_publications <- function(cv, lang) {
  bdg_lbls <- list(
    book = c(eu="LIBURUA", en="BOOK",  es="LIBRO"),
    rpkg = c(eu="R PKG",   en="R PKG", es="PKG R")
  )
  pubs <- sapply(cv$publications, function(p) {
    bdg <- if (!is.null(p$badge) && p$badge != "")
      badge_html(p$badge, bdg_lbls[[p$badge]][[lang]]) else ""
    trans_field <- switch(lang,
      eu = if (!is.null(p$translation_eu)) p$translation_eu else "",
      es = if (!is.null(p$translation_es)) p$translation_es else "",
      "")
    trans_html <- if (nchar(trans_field) > 0)
      sprintf('<div class="pub-translation">%s</div>', trans_field) else ""
    sprintf(
      '<div class="pub">
         <div class="pub-title"><a href="%s" target="_blank">%s</a>%s</div>
         %s
         <div class="pub-meta">%s (%s). <em>%s</em>.</div>
       </div>',
      p$doi, p$title, bdg, trans_html,
      md_bold(p$authors), p$year, p$journal)
  })
  lbls <- c(eu="Argitalpen nagusiak", en="Selected publications", es="Publicaciones seleccionadas")
  sprintf('<div class="subsection-label" style="margin-top:1.4rem">%s</div>%s',
          lbls[[lang]], paste(pubs, collapse = "\n"))
}

build_kt_items <- function(items, lang) {
  paste(sapply(items, function(e) {
    bdg      <- if (!is.null(e$badge) && e$badge != "") badge_html(e$badge, tr(e$badge_label, lang)) else ""
    title_txt <- md_inline(tr(e$title, lang))
    sub_txt   <- md_inline(tr(e$subtitle, lang))
    desc_txt  <- gsub("\\n", "<br>", md_inline(tr(e$desc, lang)))
    years_txt <- tr(e$years, lang)
    sub_html  <- if (nchar(sub_txt)  > 0) sprintf('<div class="pub-meta">%s · %s</div>', sub_txt, years_txt) else
      sprintf('<div class="pub-meta">%s</div>', years_txt)
    desc_html <- if (nchar(desc_txt) > 0) sprintf('<div class="pub-meta" style="margin-top:0.3rem">%s</div>', desc_txt) else ""
    sprintf('<div class="pub"><div class="pub-title">%s%s</div>%s%s</div>',
            title_txt, bdg, sub_html, desc_html)
  }), collapse = "\n")
}

build_projects <- function(cv, lang) {
  items <- sapply(cv$research_projects, function(p) {
    bdg      <- if (!is.null(p$badge) && p$badge != "") badge_html(p$badge, tr(p$badge_label, lang)) else ""
    sub_txt  <- md_inline(tr(p$subtitle, lang))
    desc_txt <- md_inline(tr(p$desc, lang))
    sub_html  <- if (nchar(sub_txt)  > 0) sprintf('<div class="entry-sub">%s</div>',  sub_txt)  else ""
    desc_html <- if (nchar(desc_txt) > 0) sprintf('<div class="entry-desc">%s</div>', desc_txt) else ""
    sprintf('<div class="entry"><div class="entry-year">%s</div><div class="entry-body"><div class="entry-title">%s %s</div>%s%s</div></div>',
            p$years, md_inline(tr(p$title, lang)), bdg, sub_html, desc_html)
  })
  lbls <- c(eu="Ikerketa-proiektu nagusiak", en="Key research projects", es="Proyectos de investigación principales")
  sprintf('<div class="subsection-label" style="margin-top:1.4rem">%s</div>%s',
          lbls[[lang]], paste(items, collapse = "\n"))
}

build_entries <- function(items, lang) {
  paste(sapply(items, function(e) {
    bdg      <- if (!is.null(e$badge) && e$badge != "") badge_html(e$badge, tr(e$badge_label, lang)) else ""
    sub_txt  <- md_inline(tr(e$subtitle, lang))
    desc_txt <- md_inline(tr(e$desc, lang))
    sub_html  <- if (nchar(sub_txt)  > 0) sprintf('<div class="entry-sub">%s</div>',  sub_txt)  else ""
    desc_html <- if (nchar(desc_txt) > 0) sprintf('<div class="entry-desc">%s</div>', desc_txt) else ""
    sprintf('<div class="entry"><div class="entry-year">%s</div><div class="entry-body"><div class="entry-title">%s %s</div>%s%s</div></div>',
            tr(e$years, lang), md_inline(tr(e$title, lang)), bdg, sub_html, desc_html)
  }), collapse = "\n")
}

# ── Bloque completo por idioma ─────────────────────────
render_lang_block <- function(cv, lang, active = FALSE) {
  active_class <- if (active) " active" else ""
  lbls <- list(
    eu = list(s1="Profila",    s2="Ikerketa",     s3="Ezagutza-transferentzia",
              s4="Hezkuntza",  s5="Karguak eta erantzukizunak",
              kt_reports="Txostenak", kt_datalabs="Data Labak"),
    en = list(s1="Profile",    s2="Research",      s3="Knowledge Transfer",
              s4="Education",  s5="Roles &amp; responsibilities",
              kt_reports="Reports", kt_datalabs="Data Labs"),
    es = list(s1="Perfil",     s2="Investigación", s3="Transferencia del conocimiento",
              s4="Formación",  s5="Cargos y responsabilidades",
              kt_reports="Informes", kt_datalabs="Data Labs")
  )[[lang]]

  sec <- function(num, title, content)
    sprintf('<section class="cv-section"><div class="section-header"><span class="section-num">%s</span><h2 class="section-title">%s</h2></div>%s</section>',
            num, title, content)

  html <- paste0(
    sprintf('<div class="lang-block%s" id="block-%s">', active_class, lang),
    sprintf('<header class="hero"><div class="hero-name">Ainhoa <em>Vega-Bayo</em></div><p class="hero-subtitle">%s</p></header>',
            tr(cv$subtitle, lang)),
    sec("01", lbls$s1, build_profile(cv, lang)),
    sec("02", lbls$s2, paste0(build_research_lines(cv, lang), build_publications(cv, lang), build_projects(cv, lang))),
    sec("03", lbls$s3, paste0(
      sprintf('<div class="subsection-label">%s</div>', lbls$kt_reports),
      build_kt_items(cv$kt_reports, lang),
      sprintf('<div class="subsection-label" style="margin-top:1.2rem">%s</div>', lbls$kt_datalabs),
      build_kt_items(cv$kt_datalabs, lang)
    )),
    sec("04", lbls$s4, build_entries(cv$education, lang)),
    sec("05", lbls$s5, paste0(
      sprintf('<div class="subsection-label">%s</div>', lbls$distinctions),
      build_entries(cv$distinctions, lang),
      sprintf('<div class="subsection-label" style="margin-top:1.2rem">%s</div>', lbls$roles),
      build_entries(cv$roles, lang)
    )),
    sprintf('</div><!-- /block-%s -->', lang)
  )
  htmltools::HTML(html)
}

# ── Sidebar dinámica ───────────────────────────────────
render_sidebar_dynamic <- function(cv) {
  avatar <- if (!is.null(cv$photo) && nchar(cv$photo) > 0 && file.exists(cv$photo))
    sprintf('<img src="%s" alt="Foto" class="avatar">', cv$photo)
  else
    sprintf('<div class="avatar-placeholder">%s</div>', cv$initials)

  name_html <- sprintf('<div class="sidebar-name">%s</div>',
    gsub(" ", "<br>", cv$name, fixed = TRUE))

  roles_html <- paste(sapply(c("eu","en","es"), function(lang) {
    active <- if (lang == "eu") " active" else ""
    sprintf('<div class="sidebar-title lang-block%s" id="sidebar-%s">%s</div>',
            active, lang, gsub("\n", "<br>", tr(cv$role, lang)))
  }), collapse = "\n")

  contact_html <- sprintf(
    '<a href="mailto:%s" class="sidebar-link">
       <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 7-10 7L2 7"/></svg>%s</a>
     <a href="%s" class="sidebar-link" target="_blank">
       <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="12" cy="12" r="10"/><path d="M9 12h6M12 9v6"/></svg>ORCID</a>
     <a href="%s" class="sidebar-link" target="_blank">
       <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M12 14l6.16-4.06A2 2 0 0 1 21 11.76V19a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-7.24a2 2 0 0 1 2.84-1.82L12 14z"/><path d="M12 14 2.84 9.94"/><path d="m12 14 9.16-4.06"/><line x1="12" y1="4" x2="12" y2="14"/></svg>Google Scholar</a>',
    cv$email, cv$email,
    cv$orcid_url,
    cv$scholar_url)

  htmltools::HTML(paste0(
    avatar, name_html, roles_html,
    '<hr class="sidebar-divider">',
    '<div class="sidebar-section"><span class="sidebar-label">Kontaktua / Contact</span>',
    contact_html, '</div>'
  ))
}
