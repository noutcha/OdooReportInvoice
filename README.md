Facture Odoo avec Montant en lettre et coordonnées bancaire
============================================================

[![Project Status](http://opensource.box.com/badges/active.svg)](http://opensource.box.com/badges)
[![Project Status](http://opensource.box.com/badges/maintenance.svg)](http://opensource.box.com/badges)
[![Average time to resolve an issue](http://isitmaintained.com/badge/resolution/noutcha/OdooReportInvoice.svg)](http://isitmaintained.com/project/noutcha/OdooReportInvoice "Average time to resolve an issue")
[![Percentage of open issues](http://isitmaintained.com/badge/open/noutcha/OdooReportInvoice.svg)](http://isitmaintained.com/project/noutcha/OdooReportInvoice "Percentage of issues still open")
[![GPL License](https://badges.frapsoft.com/os/gpl/gpl.png?v=103)](https://opensource.org/licenses/GPL-3.0/)

En plus du montant en chiffre, ajouter le montant en lettres dans l'impression de vos Factures Odoo.
* Un papier entête comme vous l'avez facilement fait sur Word ou avec un logiciel de DAO.
* Information bancaire sur la facture.

Petit aperçus du code
=====================


```xml

<?xml version="1.0"?>
<t t-name="report_invoice_document">
    <t t-call="report.external_layout">
        <div class="page" style="font-size: 1.2em;">
            <div class="row mt32 mb32">
            <br />
            <span t-if="o.date_invoice"><strong>Date: </strong><span t-field="o.date_invoice"/></span>
              <br />
              <div class="col-xs-0" t-if="o.name">
                    <strong>Your Ref:</strong>
                    <span t-field="o.name"/>
             <br />
                <span t-if="o.origin"><strong>Our Ref: </strong><span t-field="o.origin"/></span>
                </div>
          <!-- info client -->
            <div class="col-xs-8 col-xs-offset-8" >
                    <address t-field="o.partner_id" t-field-options="{&quot;widget&quot;: &quot;contact&quot;, &quot;fields&quot;: [&quot;address&quot;, &quot;name&quot;], &quot;no_marker&quot;: true}"/>
                    <span t-if="o.partner_id.vat">TIN: <span t-field="o.partner_id.vat"/></span>
                </div>
              
                <div class="col-xs-2" t-if="o.partner_id.ref">
                    <strong>Customer Code:</strong>
                    <span  t-field="o.partner_id.ref"/>
                </div>
                
            </div>


            <h2>
                <span t-if="o.type == 'out_invoice' and (o.state == 'open' or o.state == 'paid')">Invoice</span>
                <span t-if="o.type == 'out_invoice' and o.state == 'proforma2'">PRO-FORMA <span t-if="o.number">N<sup>o</sup> <span t-field="o.number"/></span> </span>
                <span t-if="o.type == 'out_invoice' and o.state == 'draft'">Draft Invoice</span>
                <span t-if="o.type == 'out_invoice' and o.state == 'cancel'">Cancelled Invoice</span>
                <span t-if="o.type == 'out_refund'">Refund</span>
                <span t-if="o.type == 'in_refund'">Supplier Refund</span>
                <span t-if="o.type == 'in_invoice'">Supplier Invoice</span>
                <span t-field="o.number"/>
            </h2>

            
<br />

            <table class="table table-condensed" border="1px" width="100%">
                <thead>
                    <tr>
                         <th class="text-left">Item</th>
                        <th class="text-left">Description</th>
                        <th class="text-center">Quantity</th>
                        <th class="text-center">UoM</th>
                        <th class="text-right">Unit Price</th>
                        <th class="text-right">Amount</th>
                    </tr>
                </thead>
                <tbody class="invoice_tbody">
                    <tr t-foreach="o.invoice_line" t-as="l">
                
                       <td class="text-left">
                             
                          <span t-if="l.item_number"><t t-esc="l.item_number" /></span>
                      </td>
                        <td class="text-left">
                                 <span t-field="l.name"/>
                         </td>
                        <td class="text-center">
                            <span t-field="l.quantity"/>
                            
                        </td>
                         <td class="text-center" width="10%">
                            <span t-field="l.uos_id" groups="product.group_uom"/>
                        </td>
                        <td class="text-right">
                            <span t-field="l.price_unit"/>
                        </td>
                        
                        <td class="text-right">
                            <span t-field="l.price_subtotal" t-field-options="{&quot;widget&quot;: &quot;monetary&quot;, &quot;display_currency&quot;: &quot;o.currency_id&quot;}"/>
                        </td>
                    </tr>
                </tbody>
            </table>

<div class="row">
<!-- tableau pour les information Banquaire -->
<div class="col-xs-8 pull-left">
<table class="table table-condensed">
<tr class="border-black">

                            <td><strong>Bank code</strong></td>
                            
                            <td><strong>Branch Account Number</strong></td>
</tr>
<tbody>
<tr>
                            
                            <td><span t-field="o.partner_bank_id.bank_bic" /></td>
                            
                            <td><span t-field="o.partner_bank_id.acc_number" /></td>
</tr>
</tbody>
</table>
<!-- Fin tableau Banque -->
</div>
                <div class="col-xs-4 pull-right">
                    <table class="table table-condensed">

                        <tr class="border-black">
                            <td><strong>Total Without Taxes</strong></td>
                            <td class="text-right">
                                <span t-field="o.amount_untaxed" t-field-options="{&quot;widget&quot;: &quot;monetary&quot;, &quot;display_currency&quot;: &quot;o.currency_id&quot;}"/>
                            </td>
                        </tr>
                        <tr>
                            
                            <td>TVA 19,25%</td>
                            <td class="text-right">
                                <span t-field="o.amount_tax" t-field-options="{&quot;widget&quot;: &quot;monetary&quot;, &quot;display_currency&quot;: &quot;o.currency_id&quot;}"/>
                            </td>
                        </tr>
                        <tr class="border-black">
                            <td><strong>Total TTC</strong></td>
                            <td class="text-right">
                                 <span t-field="o.amount_total" t-field-options="{&quot;widget&quot;: &quot;monetary&quot;, &quot;display_currency&quot;: &quot;o.currency_id&quot;}"/>
                            </td>
                        </tr>
                       





<t t-if="o.state == 'open' or o.state == 'paid'">
<tr class="border-black">
                <td>Paid to date</td>
                <td class="text-right">
                    <span t-esc="o.amount_total-o.residual" t-esc-options="{&quot;widget&quot;: &quot;monetary&quot;, &quot;display_currency&quot;: &quot;o.currency_id&quot;}"/>
                </td>
            </tr>
            <tr>
                <td><strong>Balance amount</strong></td>
                <td class="text-right">
                    <span t-field="o.residual" t-field-options="{&quot;widget&quot;: &quot;monetary&quot;, &quot;display_currency&quot;: &quot;o.currency_id&quot;}"/>
                </td>
            </tr>
</t>

```

aperçus du resultat
=======
![Resultat](https://i.ibb.co/VgTxM1C/invoice.jpg "Invoice Factures")


License
======
Double licence sous les licences MIT ou GPL version 2.


Contribuant
===========
**Michel NOUTCHA**  :wink:

<a href="https://ibb.co/HBs68F3"><img src="https://i.ibb.co/HBs68F3/ntm.jpg" width="50" height="50" alt="Michel NOUTCHA" border="0"></a>
