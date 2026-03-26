---
title: "Changing Og Locale Output"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Api
    - Opengraph
    - Features
---


当您在 Yoast SEO 中启用 OpenGraph 设置时，它会输出一个 `og:locale` 元标签。这个 `og:locale` 基于您 WordPress 安装的语言设置。

因此，如果您希望它输出 `nl_NL` 而非默认的 `en_US`，只需将 WordPress 安装的语言切换为 `nl_NL`。您可以在 [Make WordPress 多语言团队页面](https://make.wordpress.org/polyglots/teams/)上阅读更多相关信息。

## 修改 `og:locale` 标签
如果您不想更改 WordPress 安装的语言，但*确实*想要更改 Yoast SEO 的区域设置输出，可以通过过滤输出来实现。您应该将如下所示的代码添加到主题的 `functions.php` 文件中：

```php
/**
 * 更改区域设置输出。
 * 
 * @param string $locale 当前区域设置。
 * 
 * @return string 区域设置。
 */
function yst_wpseo_change_og_locale( $locale ) {
    return 'nl_NL';
}

add_filter( 'wpseo_locale', 'yst_wpseo_change_og_locale' );
```

## 移除 `og:locale` 标签
如果您希望移除 `og:locale` 标签，需要通过将以下代码添加到您主题的 `functions.php` 文件中，从 `wpseo_opengraph` 中解除 `WPSEO_OpenGraph->locale()` 的挂钩。

此代码片段在大多数情况下有效，但如果您的服务器使用 PHP 5.2，它将无法工作。为了您网站的健康与安全，我们建议在进行任何代码更改之前备份您的网站和数据库，因为我们无法保证在您的特定网站上会发生什么。

```php
function remove_locale_presenter( $presenters ) {
    return array_map( function( $presenter ) {
        if ( ! $presenter instanceof Yoast\WP\SEO\Presenters\Open_Graph\Locale_Presenter ) {
            return $presenter;
        }
    }, $presenters );
}

add_action( 'wpseo_frontend_presenters', 'remove_locale_presenter' );
```

## 有效的 `og:locale` 选项
OpenGraph 有一个[有限的受支持 `og:locale` 指定列表](https://developers.facebook.com/docs/javascript/internationalization#locales)。如果您的区域设置不在此列表中，我们的插件将输出与您语言最匹配的选项。

截至 2018 年 7 月，以下是[有效的 `og:locale` 选项](https://developers.facebook.com/docs/reference/opengraph/object-type/article/)：

`en_us`, `ca_es`, `cs_cz`, `cx_ph`, `cy_gb`, `da_dk`, `de_de`, `eu_es`, `en_pi`, `en_ud`, `ck_us`, `es_la`, `es_es`, `es_mx`, `gn_py`, `fi_fi`, `fr_fr`, `gl_es`, `ht_ht`, `hu_hu`, `it_it`, `ja_jp`, `ko_kr`, `nb_no`, `nn_no`, `nl_nl`, `fy_nl`, `pl_pl`, `pt_br`, `pt_pt`, `ro_ro`, `ru_ru`, `sk_sk`, `sl_si`, `sv_se`, `th_th`, `tr_tr`, `ku_tr`, `zh_cn`, `zh_hk`, `zh_tw`, `fb_lt`, `af_za`, `sq_al`, `hy_am`, `az_az`, `be_by`, `bn_in`, `bs_ba`, `bg_bg`, `hr_hr`, `nl_be`, `en_gb`, `eo_eo`, `et_ee`, `fo_fo`, `fr_ca`, `ka_ge`, `el_gr`, `gu_in`, `hi_in`, `is_is`, `id_id`, `ga_ie`, `jv_id`, `kn_in`, `kk_kz`, `ky_kg`, `la_va`, `lv_lv`, `li_nl`, `lt_lt`, `mi_nz`, `mk_mk`, `mg_mg`, `ms_my`, `mt_mt`, `mr_in`, `mn_mn`, `ne_np`, `pa_in`, `rm_ch`, `sa_in`, `sr_rs`, `so_so`, `sw_ke`, `tl_ph`, `ta_in`, `tt_ru`, `te_in`, `ml_in`, `uk_ua`, `uz_uz`, `vi_vn`, `xh_za`, `zu_za`, `km_kh`, `tg_tj`, `ar_ar`, `he_il`, `ur_pk`, `fa_ir`, `sy_sy`, `yi_de`, `qc_gt`, `qu_pe`, `ay_bo`, `se_no`, `ps_af`, `tl_st`, `gx_gr`, `my_mm`, `qz_mm`, `or_in`, `si_lk`, `rw_rw`, `ak_gh`, `nd_zw`, `sn_zw`, `cb_iq`, `ha_ng`, `yo_ng`, `ja_ks`, `lg_ug`, `br_fr`, `zz_tr`, `tz_ma`, `co_fr`, `ig_ng`, `as_in`, `am_et`, `lo_la`, `ny_mw`, `wo_sn`, `ff_ng`, `sc_it`, `ln_cd`, `tk_tm`, `sz_pl`, `bp_in`, `ns_za`, `tn_bw`, `st_za`, `ts_za`, `ss_sz`, `ks_in`, `ve_za`, `nr_za`, `ik_us`, `su_id`, `om_et`, `em_zm`, `qr_gr` 和 `iu_ca`