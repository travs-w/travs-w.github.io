---
layout: post
title:  Custom Button to Send Email to Partner
date:   2015-05-05 16:40:16
description: email your customer directly from their contact record
---

I felt that Odoo was missing a key feature, so I decided to add it in myself. I wanted to be able to email a contact directly from their partner record.

It's a fairly simple change, consisting of two basic parts:

<ul>
    <li>Adding the button to the form</li>
    <li>Adding the email function to the button</li>
</ul>

<blockquote>
    <strong>This post assumes you know the basics of an Odoo module's structure.</strong>
</blockquote>

First things first, we can add the button to the form with the following.

<strong>views/partner.xml</strong>

{% highlight xml %}

<?xml version="1.0" encoding="UTF-8"?>
<openerp>
    <data>
        <record id="res_partner_email_header_form" model="ir.ui.view">
            <field name="name">res.partner.header</field>
            <field name="model">res.partner</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="arch" type="xml">
                <xpath expr="//form/sheet" position="before">
                    <button name="email_partner" type="object" string="Create Email" class="oe_highlight"/>
                </xpath>
            </field>
        </record>
    </data>
</openerp>

{% endhighlight %}

If you install the module and go to your partner form, you should have a "Create Email" button at the top of your form.

<blockquote class="caption left">
    If your button does not appear, try refreshing your browser window.
</blockquote>

<div class="center">
    <a href="{{ site.baseurl }}/img/email_partner.png"><img src="{{ site.baseurl }}/img/email_partner.png" alt="" title="Create Email button at top of partner form" style="max-height: 100%; max-width: 100%;"></a>
</div>

<br/>
Of course, you'll notice that if you click the button, then you get an error. That's because we haven't given any meaning to the email_partner function.

<strong>models/partner.py</strong>

{% highlight python %}

from openerp import models, fields, api


class res_partner(models.Model):
    _inherit = 'res.partner'

    @api.multi
    def email_partner(self):
        '''
        This function opens a window to compose an email, with the edi sale template message loaded by default
        '''
        self.ensure_one()
        ir_model_data = self.env['ir.model.data']
        try:
            compose_form_id = ir_model_data.get_object_reference('mail', 'email_compose_message_wizard_form')[1]
        except ValueError:
            compose_form_id = False

        # It's worth noting that Odoo 9 uses 'mail.template' whereas Odoo 8 uses 'email.template'
        template_id = self.env['mail.template'].search([('name', '=', 'Your Email Template Name')], limit=1)
        ctx = dict()
        ctx.update({
            'default_model': 'res.partner',
            'default_res_id': False,
            'default_use_template': True,
            'default_template_id': template_id.id or False,
            'default_composition_mode': 'comment',
            'skip_notification': True,
        })
        return {
            'type': 'ir.actions.act_window',
            'view_type': 'form',
            'view_mode': 'form',
            'res_model': 'mail.compose.message',
            'views': [(compose_form_id, 'form')],
            'view_id': compose_form_id,
            'target': 'new',
            'context': ctx,
        }

{% endhighlight %}

<blockquote>Be sure to replace 'Your Email Template Name' with the appropriate name for your needs. The template provided will be loaded automatically.</blockquote>

Update your module and <em>voila!</em> You'll get a popup window to compose your email and send it off!

In order to get the most out of your module, you'll want to make sure your email template is set up appropriately.

You can include special settings in the Email Template configuration to auto-fill the partner's email, saving you a step and preventing any errors. 

<blockquote>Try using the Dynamic Placeholder Generator to help create dynamic values for your email template body, subject, and more!</blockquote>

<div class="center">
    <a href="{{ site.baseurl }}/img/email_partner_template.png"><img src="{{ site.baseurl }}/img/email_partner_template.png" alt="" title="Create Email button at top of partner form" style="max-height: 100%; max-width: 100%;"></a>
</div>

<br/>
You can view my source code over on <a href="https://github.com/{{ site.github_username }}/email_partner" title="Email Partner on Github" target="_blank">my Github repository</a>, but <em>be warned - it has a few minor customizations, which will not work with a default Odoo installation</em>. Happy coding!


<!-- This theme implements a built-in Jekyll feature, the use of Pygments, for sytanx highlighting. It supports more than 100 languages. This example is in C++. All you have to do is wrap your code in a liquid tag: 
{% raw  %}
{% highlight c++ %}  <br/> code code code <br/> {% endhighlight %}{% endraw %} -->
